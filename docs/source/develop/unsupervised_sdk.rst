DaoAI Unsupervised Defect Detection SDK
========================================

The DaoAI Unsupervised Defect Detection SDK provides a comprehensive set of tools to help users load pretrained models for inference or train custom models using user-provided image data for defect detection.

You can also explore our GitHub repository, which contains the C++ example for unsupervised defect segmentation.

Link: `DaoAI World SDK Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_

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

   #include <dlsdk/utils.h>
   #include <dlsdk/model.h>
   #include <iostream>
   #include <fstream>

   using namespace DaoAI::DeepLearning;

   int main()
   {
      try {
         // Initialize Unsupervised library
         initialize();

         // Configure the model and data path
         std::string root_directory = "C:/Users/daoai/test_vision/";  // Change to your own directory

         // Construct the model on the specified device
         Vision::UnsupervisedDefectSegmentation model(DeviceType::GPU);
         model.addComponentArchive(root_directory + "unsup_img_whole.dwm");
         std::cout << model.getBatchSize() << std::endl;

         // Set batch size
         model.setBatchSize(1);

         std::string img_path = root_directory + "unsup_img_whole (1).png";  // Change to your own directory
         Image img(img_path);

         Vision::UnsupervisedDefectSegmentationResult result = model.inference(img);

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

      Vision::UnsupervisedDefectSegmentation model(DeviceType::GPU);
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

      Vision::UnsupervisedDefectSegmentationResult result = model.inference(img);
      std::cout << "Anomaly score: " << result.confidence << std::endl;
      std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n\n";

   **Function**: Performs inference on the image and outputs the anomaly score and results in JSON format.

   .. note::

      The AI Deviation Score represents how anomalous the model considers the current sample, ranging from 0 to 1.

      A score of 0 indicates a standard (normal) sample, and values close to 0 indicate high similarity to normal samples.

      A score of 1 indicates an anomalous sample, and values close to 1 suggest greater deviation from normal samples.

      The model will automatically set a reasonable threshold based on the training set.
      If needed, you can also manually add a condition to define a custom threshold.

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
---------------------------------------------

The following code demonstrates how to train a model with user-provided sample data and perform inference:

This part uses the OpenCV library to draw masks. Please make sure OpenCV is installed.

Refer to the GitHub repository for the code: `Unsupervised Pixel-level Example Code <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo/blob/2.24.8.0/cpp_demos/DW_SDK_Cpp_Example/DW%20UnsupervisedDefectSegmentation/main.cpp>`_

Function: Train a model using sample data provided by the user, and perform inference using the trained model.

Usage
^^^^^

1. **Launch the application** or start debugging in Visual Studio

2. **Provide image folder path**
   Enter the folder path containing the images (supports `.png`, `.jpg`, `.jpeg` formats).

3. **Interactive annotation interface**
   Use the GUI to annotate the images:

- **Keyboard controls**:

  - `n`: Switch to the next image
  - `p`: Go back to the previous image
  - `g`: Mark the image as GOOD
  - `b`: Mark the image as BAD (allows polygon annotation)
  - `r`: Reset the polygon
  - `f`: Complete the polygon (connect the last point to the first)
  - `q`: Exit the annotation tool

- **Mouse controls**:

  - **Left click**: Add a polygon point on the image (for BAD images)
  - **Mouse wheel**: Zoom in/out

4. **Save annotation results**
   After exiting the annotation interface, the tool automatically performs the following actions:

- Copy GOOD images to the `out/good` directory.
- Copy BAD images to the `out/bad` directory.
- Generate binary masks for BAD images and save them in the `out/masks` directory.

5. **Load annotations for training**
   The tool reloads the annotated images and masks from the `out` directory to prepare training data.

6. **Build training component**
   The tool uses DaoAI's **UnsupervisedDefectSegmentation** model to build a training component based on the annotations.

7. **Model inference**
   Use the training component to perform inference on new image data and output defect detection results.

Summary
-------

With the DaoAI Unsupervised Defect Detection SDK, users can easily load pretrained models for efficient inference or use their own data for custom training to meet specific needs.
