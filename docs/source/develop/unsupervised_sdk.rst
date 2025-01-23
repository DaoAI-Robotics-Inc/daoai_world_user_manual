DaoAI Unsupervised Defect Detection SDK
========================================

The DaoAI Unsupervised Defect Detection SDK provides a comprehensive set of tools to help users load pretrained models for inference or train custom models using user-provided image data for defect detection.

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

    #include <anomaly_fast/anomaly_fast.h>
    #include <anomaly_fast/models/unsupervised_defect_segmentation.h>
    #include <iostream>
    #include <fstream>

    using namespace DaoAI::AnomalyFast;

    int main2()
    {
        try {
            // Initialize Anomaly Fast library
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

1. **Initialize the Anomaly Fast Library**

   .. code-block:: cpp

      initialize();

   **Function**: Initializes the Anomaly Fast library to prepare for model loading and inference operations.

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

Second Part: Training Custom Models for Inference
-------------------------------------------------

The following code demonstrates how to train a custom model using user-provided sample data and perform inference:

.. code-block:: cpp

    // Code from main function here...

Code Explanation
^^^^^^^^^^^^^^^^

1. **Initialize Anomaly Fast Library**: Same as above.

2. **Set Model and Data Paths**: Configures paths for training data and components.

3. **Load "Good" Sample Images**: Reads and stores "good" sample images for training.

4. **Load "Bad" Sample Images and Masks**: Reads "bad" samples and generates corresponding binary masks.

5. **Train Model and Save Components**: Trains the model and saves the trained components.

6. **Perform Inference and Output Results**: Uses the trained model for inference.

7. **Exception Handling**: Captures any errors during execution.

Summary
-------

Using the DaoAI Unsupervised Defect Detection SDK, users can easily load pretrained models for efficient inference or train custom models using their own data to meet specific requirements.
