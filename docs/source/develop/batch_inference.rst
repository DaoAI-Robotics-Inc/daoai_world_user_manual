Accelerating Inference with Batch Inference
============================================

This document describes how to implement two different model inference methods using the DaoAI Deep Learning SDK and compares their performance.
The goal is to demonstrate the performance improvement of using **Batch Inference** compared to traditional **multi-threaded** single-image inference.

.. contents::
    :local:

Code Structure
--------------

The code mainly consists of two core functions:

1. **normal_inference():**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This function uses the traditional multi-threading method to perform inference on each image one by one.
In each batch, one thread is created per image — a total of 16 threads are created per batch to process in parallel.

By measuring the processing time of each batch (excluding the first and last batches), the average inference time per image is calculated.

Example code:

.. code-block:: cpp

    int normal_inference()
    {
        try
        {
            DaoAI::DeepLearning::initialize();

            std::string root = "C:/Users/daoai/Downloads/";
            std::string model_zip_path = root + "dami_fast.dwm";
            std::string images_path = root + "dami_data/";
            std::string out_path = images_path + "out/";

            if (!std::filesystem::exists(out_path))
            {
                std::filesystem::create_directory(out_path);
            }

            std::vector<std::string> image_files;
            for (const auto& entry : std::filesystem::directory_iterator(images_path))
            {
                if (entry.is_regular_file())
                {
                    auto ext = entry.path().extension().string();
                    std::transform(ext.begin(), ext.end(), ext.begin(), ::tolower);
                    if (ext == ".png" || ext == ".jpg" || ext == ".jpeg")
                    {
                        image_files.push_back(entry.path().string());
                    }
                }
            }

            DaoAI::DeepLearning::Vision::Classification model(model_zip_path, DaoAI::DeepLearning::DeviceType::GPU);
            const size_t batch_size = 16;
            size_t num_batches = (image_files.size() + batch_size - 1) / batch_size;

            double total_inference_time_excluding = 0.0;
            int total_images_excluding = 0;

            for (size_t batch_idx = 0; batch_idx < num_batches; ++batch_idx)
            {
                auto batch_start_time = std::chrono::high_resolution_clock::now();

                std::vector<std::thread> threads;
                size_t start_index = batch_idx * batch_size;
                size_t end_index = std::min(start_index + batch_size, image_files.size());

                for (size_t i = start_index; i < end_index; ++i)
                {
                    threads.emplace_back([&, i]() {
                        try
                        {
                            DaoAI::DeepLearning::Image daoai_image(image_files[i]);
                            auto prediction = model.inference(daoai_image);
                        }
                        catch (const std::exception& e)
                        {
                            std::cerr << "Error processing image " << image_files[i] << ": " << e.what() << "\n";
                        }
                    });
                }

                for (auto& t : threads)
                {
                    if (t.joinable())
                        t.join();
                }

                auto batch_end_time = std::chrono::high_resolution_clock::now();
                std::chrono::duration<double, std::milli> batch_duration = batch_end_time - batch_start_time;
                std::cout << "Batch " << (batch_idx + 1) << " took " << batch_duration.count() << " ms" << std::endl;

                if (batch_idx != 0 && batch_idx != num_batches - 1)
                {
                    total_inference_time_excluding += batch_duration.count();
                    total_images_excluding += (end_index - start_index);
                }
            }

            if (total_images_excluding > 0)
            {
                double avg_time_per_image = total_inference_time_excluding / total_images_excluding;
                std::cout << "Average inference time per image (excluding first and last batch): "
                    << avg_time_per_image << " ms" << std::endl;
            }
        }
        catch (const std::exception& e)
        {
            std::cerr << "Exception occurred: " << e.what() << "\n";
            return 1;
        }
        return 0;
    }

2. **batch_inference():**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This function demonstrates how to use the SDK's built-in batch inference capability for performance optimization.
Unlike manually creating threads, batch inference allows the model to process multiple images in a single inference call.

Using a batch size of 16 as an example, the function calculates the total inference time and the average inference time per image for the entire dataset.

Example code:

.. code-block:: cpp

    int batch_inference()
    {
        try
        {
            DaoAI::DeepLearning::initialize();

            std::string root = "C:/Users/daoai/Downloads/";
            std::string model_zip_path = root + "dami_fast.dwm";
            std::string images_path = root + "dami_data/";

            DaoAI::DeepLearning::Vision::Classification model(model_zip_path, DaoAI::DeepLearning::DeviceType::GPU);
            model.setBatchSize(16);

            std::vector<DaoAI::DeepLearning::Image> images;
            for (const auto& entry : std::filesystem::directory_iterator(images_path))
            {
                if (entry.is_regular_file())
                {
                    auto ext = entry.path().extension().string();
                    std::transform(ext.begin(), ext.end(), ext.begin(), ::tolower);
                    if (ext == ".png" || ext == ".jpg" || ext == ".jpeg")
                    {
                        images.emplace_back(entry.path().string());
                    }
                }
            }

            auto start = std::chrono::high_resolution_clock::now();
            auto prediction = model.inference(images);
            auto end = std::chrono::high_resolution_clock::now();

            std::chrono::duration<double, std::milli> inference_time = end - start;
            std::cout << "Total inference time for " << images.size() << " images: "
                << inference_time.count() << " ms" << std::endl;

            double per_image_time = inference_time.count() / images.size();
            std::cout << "Average inference time per image: "
                << per_image_time << " ms" << std::endl;

        }
        catch (const std::exception& e)
        {
            std::cerr << "Exception occurred: " << e.what() << std::endl;
            return 1;
        }
        return 0;
    }

Performance Comparison
----------------------

Using a classification model as an example:

.. list-table::
   :header-rows: 1

   * - Mode Type
     - Hardware
     - Image Resolution
     - Average Inference Time per Image (Standard Inference)
     - Average Inference Time per Image (Batch Inference)
   * - Fast
     - 2080 Super
     - 96×96
     - 19.17ms
     - 2.9ms
   * - Balanced
     - 2080 Super
     - 96×96
     - 11.45ms
     - 3.05ms
   * - Accurate
     - 2080 Super
     - 96×96
     - 25.72ms
     - 3.97ms

For the full test table, please refer to :ref:`Inference Time Table for Models of Different Sizes`
