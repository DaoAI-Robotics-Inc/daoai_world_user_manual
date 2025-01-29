DaoAI Unsupervised Defect Detection SDK
========================================

The DaoAI Unsupervised Defect Detection SDK provides a comprehensive set of tools to help users load pretrained models for inference or train custom models using user-provided image data for defect detection.

DaoAI Unsupervised Defect Detection SDK is only available in C++

Installation and Preparation
----------------------------

Before using the SDK, ensure you have downloaded and installed the necessary packages:

Download the `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_, and find the installation package ZIP file for **2.22.8.0** in the **Unsupervised Defect Segmentation SDK** directory.

Feature Overview
----------------

1. **Load Pretrained Models for Inference** (see `main2` function)  
   Using pretrained models downloaded from the DaoAI World platform, the SDK can perform the following:

   - Supports pixel-level and image-level inference.
   - Provides inference results, including anomaly scores and annotations.

2. **Train Custom Models for Inference** (see `main` function)  
   Users can train a pixel-level or image-level defect detection model by providing good and bad sample data.

Usage Guide
-----------

The following code demonstrates how to load a pretrained model downloaded from the DaoAI World platform and use it for image inference. The code components are explained in detail below:

First Part: Loading Pretrained Models for Inference
---------------------------------------------------

The following code shows how to load a pretrained model and perform inference on an image:

.. code-block:: cpp

   #include <daoai_unsupervised/daoai_unsupervised.h>
   #include <daoai_unsupervised/models/unsupervised_defect_segmentation.h>
   #include <iostream>
   #include <fstream>

   using namespace DaoAI::Unsupervised;

   int main()
   {
      try {
         // Initialize Unsupervised library
         initialize();

         // Configure the model and data path
         std::string root_directory = "C:/Users/daoai/test_vision/";  // Change to your own directory

         // Construct the model on the specified device
         UnsupervisedDefectSegmentation model(DeviceType::GPU);
         model.addComponentArchive(root_directory + "unsup_img_whole.dwm");
         std::cout << model.getBatchSize() << std::endl;

         // Set batch size
         model.setBatchSize(1);

         std::string img_path = root_directory + "unsup_img_whole (1).png";  // Change to your own directory
         Image img(img_path);

         UnsupervisedDefectSegmentationResult result = model.inference(img);

         // Print the result
         std::cout << "Anomaly score: " << result.confidence << std::endl;
         std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n\n";

         // Save the result to a file
         std::string file_path = root_directory + "output.json";
         std::ofstream output_file(file_path);
         if (output_file.is_open()) {
               output_file << result.toAnnotationJSONString();
               output_file.close();
               std::cout << "JSON result saved to: " << file_path << std::endl;
         } else {
               std::cerr << "Failed to open the file: " << file_path << std::endl;
         }

         return 0;
      }
      catch (const std::exception& e) {
         std::cout << "Caught an exception: " << e.what() << std::endl;
         return -1;
      }
   }

Code Explanation
^^^^^^^^^^^^^^^^

1. **Initialize the Unsupervised Library**

   .. code-block:: cpp

      initialize();

   **Function**: Initializes the Unsupervised library to prepare for model loading and inference operations.

2. **Configure Model and Data Paths**

   .. code-block:: cpp

      std::string root_directory = "C:/Users/daoai/test_vision/";

   **Function**: Sets the root directory path for the model and image files. Modify as needed.

3. **Load Pretrained Model**

   .. code-block:: cpp

      UnsupervisedDefectSegmentation model(DeviceType::GPU);
      model.addComponentArchive(root_directory + "unsup_img_whole.dwm");

   **Function**: Loads the pretrained model component and supports GPU devices for enhanced inference performance.

4. **Set Batch Size**

   .. code-block:: cpp

      model.setBatchSize(1);

   **Function**: Sets the batch size for model inference. Here, it is set to 1.

5. **Load Input Image**

   .. code-block:: cpp

      std::string img_path = root_directory + "unsup_img_whole (1).png";
      Image img(img_path);

   **Function**: Loads the image file for inference.

6. **Perform Inference and Output Results**

   .. code-block:: cpp

      UnsupervisedDefectSegmentationResult result = model.inference(img);
      std::cout << "Anomaly score: " << result.confidence << std::endl;
      std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n\n";

   **Function**: Performs inference on the image and outputs the anomaly score and results in JSON format.

7. **Save Inference Results**

   .. code-block:: cpp

      std::ofstream output_file(file_path);
      if (output_file.is_open()) {
          output_file << result.toAnnotationJSONString();
          output_file.close();
          std::cout << "JSON result saved to: " << file_path << std::endl;
      } else {
          std::cerr << "Failed to open the file: " << file_path << std::endl;
      }

   **Function**: Saves the inference results as a JSON file for further analysis.

8. **Exception Handling**

   .. code-block:: cpp

      catch (const std::exception& e) {
          std::cout << "Caught an exception: " << e.what() << std::endl;
          return -1;
      }

   **Function**: Catches any exceptions and outputs error messages.

Part 2: Self-training the Model and Inference
--------------------------------------------

The following code demonstrates how to use the user's provided sample data to self-train a model and perform inference:

.. code-block:: cpp

   #include <daoai_unsupervised/daoai_unsupervised.h>
   #include <daoai_unsupervised/models/unsupervised_defect_segmentation.h>
   #include <iostream>
   #include <fstream>
   #include <opencv2/opencv.hpp>

   using namespace DaoAI::Unsupervised;

   int main()
   {
      try {
         // Initialize Unsupervised library
         initialize();

         // Configure the model and data path
         std::string root_directory = "C:/Users/daoai/test_vision/";  // Change to your own directory
         std::string data_path = "C:/Users/daoai/test_vision/ano/";  // Change to your own data directory

         // Load images
         std::vector<Image> good_images;
         for (auto& file : std::filesystem::directory_iterator(data_path + "good"))
         {
               if (file.path().extension() == ".png")
               {
                  good_images.push_back(Image(file.path().string()));
               }
         }

         std::vector<Image> bad_images;
         std::vector<Image> masks;
         for (auto& file : std::filesystem::directory_iterator(data_path + "bad"))
         {
               if (file.path().extension() == ".png")
               {
                  Image image(file.path().string());
                  bad_images.push_back(image);

                  // Create a binary mask
                  cv::Mat maskMat = cv::Mat::zeros(image.height, image.width, CV_8UC1);
                  int centerX = image.width / 2;
                  int centerY = image.height / 2;
                  int radius = static_cast<int>(image.width * 0.25);
                  cv::circle(maskMat, cv::Point(centerX, centerY), radius, cv::Scalar(255), -1);

                  Image mask(maskMat.rows, maskMat.cols, DaoAI::Unsupervised::Image::Type::GRAYSCALE, maskMat.data);
                  masks.push_back(mask.clone());
               }
         }

         // Construct the model
         UnsupervisedDefectSegmentation model(DeviceType::GPU);
         model.setDetectionLevel(DetectionLevel::PIXEL);

         ComponentMemory component;
         try
         {
               component = model.createComponentMemory("screw", good_images, bad_images, masks, true);
               component.save(data_path + "component_1.pth");
               model.setBatchSize(1);
         }
         catch (std::exception& e)
         {
               std::cout << e.what() << "\n";
         }

         UnsupervisedDefectSegmentationResult result = model.inference(bad_images[0]);

         std::cout << "Anomaly score: " << result.confidence << std::endl;
         std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n";
         return 0;
      }
      catch (const std::exception& e) {
         std::cout << "Caught an exception: " << e.what() << std::endl;
         return -1;
      }
   }

Code Function: The code uses the user's provided sample data to train the model and perform inference using the trained model.

Code Explanation
^^^^^^^^^

1. **Initialize Unsupervised Library**

   .. code-block:: cpp

      initialize();

   **Function**: Similar to before, this initializes the Unsupervised library.

2. **Set Model and Data Paths**

   .. code-block:: cpp

      std::string root_directory = "C:/Users/daoai/test_vision/";
      std::string data_path = "C:/Users/daoai/test_vision/ano/";

   **Function**: Sets the paths where the model files and sample data are stored.

3. **Load “Good” Sample Images**

   .. code-block:: cpp

      for (auto& file : std::filesystem::directory_iterator(data_path + "good")) {
          if (file.path().extension() == ".png") {
              good_images.push_back(Image(file.path().string()));
          }
      }

   **Function**: Loads the "good" sample images into memory for model training.

4. **Load “Bad” Sample Images and Masks**

   .. code-block:: cpp

      for (auto& file : std::filesystem::directory_iterator(data_path + "bad")) {
          if (file.path().extension() == ".png") {
              Image image(file.path().string());
              bad_images.push_back(image);

              // Create a binary mask
              cv::Mat maskMat = cv::Mat::zeros(image.height, image.width, CV_8UC1);
              int centerX = image.width / 2;
              int centerY = image.height / 2;
              int radius = static_cast<int>(image.width * 0.25);
              cv::circle(maskMat, cv::Point(centerX, centerY), radius, cv::Scalar(255), -1);

              Image mask(maskMat.rows, maskMat.cols, DaoAI::Unsupervised::Image::Type::GRAYSCALE, maskMat.data);
              masks.push_back(mask.clone());
          }
      }

   **Function**: Loads the "bad" sample images and generates corresponding binary masks for the training process.

5. **Construct and Train the Model**

   .. code-block:: cpp

      ComponentMemory component = model.createComponentMemory("screw", good_images, bad_images, masks, true);
      component.save(data_path + "component_1.pth");

   **Function**: Trains the model using the provided sample data and saves the trained model component.

   The `true` argument at the end indicates that the model will be loaded into memory after training. If set to `false`, the model will not be loaded into memory, and `addComponentMemory(file_path)` will be needed to load the trained model for inference.

6. **Inference and Output Results**

   .. code-block:: cpp

      UnsupervisedDefectSegmentationResult result = model.inference(bad_images[0]);
      std::cout << "Anomaly score: " << result.confidence << std::endl;
      std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n";

   **Function**: Performs inference using the trained model on a test image and outputs the anomaly score and the result in JSON format.

7. **Exception Handling**

   .. code-block:: cpp

      catch (const std::exception& e) {
          std::cout << "Caught an exception: " << e.what() << std::endl;
          return -1;
      }

   **Function**: Catches any exceptions that may occur and prints the error message.

Summary
-------

Using DaoAI’s unsupervised defect detection SDK, users can easily load pre-trained models for efficient inference or train their own models using custom data to meet specific needs.
