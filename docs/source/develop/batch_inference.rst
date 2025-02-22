使用Batch Inference提速推理
============================================

本文件介绍了如何通过 DaoAI 深度学习 SDK 实现两种不同的模型推理方法，并比较它们的性能。
目标是展示使用批量推理 (Batch Inference) 相较于传统多线程 (Multi-threading) 单张图片推理的性能提升。


.. contents::
    :local:
    

代码结构
--------

代码主要由两个核心函数组成：

1. **normal_inference()**:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

该函数使用传统的多线程方法逐张图片进行推理。
在每一个批次中为每张图片创建一个线程，每个批次创建16个线程，并行处理。

通过测量每个批次的处理时间（排除首尾两个批次），计算平均每张图片的推理时间。

示例代码:

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

2. **batch_inference()**:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

该函数演示了如何使用 SDK 内置的批量推理功能进行性能优化。
与手动创建线程不同，批量推理允许模型在一次推理调用中处理多个图片。

批次大小以16为例，函数计算整个图片集的总推理时间和平均推理时间。

示例代码:

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

性能对比
--------

以分类模型为例 

.. list-table::
   :header-rows: 1

   * - 模式类型
     - 硬件
     - 图像尺寸大小
     - 平均一张图的推理时间（普通单张推理）
     - 平均一张图的推理时间（内置批次推理）
   * - 快速
     - 2080 Super
     - 96*96
     - 19.17ms
     - 2.9ms
   * - 均衡
     - 2080 Super
     - 96*96
     - 11.45ms
     - 3.05ms
   * - 准确
     - 2080 Super
     - 96*96
     - 25.72ms
     - 3.97ms 
         

完整的测试表格请参考 :ref:`各模型、各模型类型、不同尺寸模型推理的运行时间表`