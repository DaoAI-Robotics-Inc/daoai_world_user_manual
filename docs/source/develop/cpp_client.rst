C++ Inference Client Example Project
====================================

This chapter provides a detailed introduction to the C++ Inference Client code example included in the DaoAI World SDK.

Library Inclusion
-----------------

In the C++ example, we include the following header files:

.. code-block:: C++

    #include <iostream>
    #include <inference_client/model.h>
    #include <inference_client/common.h>
    #include <string>
    #include <fstream>

Reading an Image
----------------

The model prediction function in the C++ Inference Client requires the image to be represented as a base64-encoded image (`base64_encoded_image`). You can use OpenCV library functions for conversion or use the online tool https://base64.guru/converter/encode/image for this.

First, define the file path. The file format is a text file with the image converted to base64 encoding, which is then read.

.. code-block:: C++

    // Image path in the local file system
    std::string image_path = "C:/Users/daoai/test_vision/kp.txt";
    std::string base64_encoded_image;
    std::ifstream fin(image_path);
    if (!fin.is_open())
    {
        std::cerr << "Failed to open file: " << image_path << "\n";
        return 1;
    }
    fin >> base64_encoded_image;
    fin.close();

You can also use the OpenCV library to read the image in base64-encoded format.

.. code-block:: C++

    std::string base64ImageEncoding(const Image& image)
    {
        cv::Mat cv_mat = Utils::image2cv(image);
        std::vector<uchar> buffer;
        cv::imencode(".png", cv_mat, buffer);
        std::string buffer_string(reinterpret_cast<const char*>(buffer.data()), buffer.size());
        std::string base64_string = base64Encode(buffer_string);
        return base64_string;
    }

    Image base64ImageDecoding(const std::string& base64String)
    {
        std::string buffer_string = base64Decode(base64String);
        std::vector<uchar> buffer(buffer_string.begin(), buffer_string.end());
        cv::Mat cv_mat = cv::imdecode(buffer, cv::IMREAD_ANYCOLOR);
        if (cv_mat.empty())
        {
            throw std::runtime_error("Failed to decode image");
        }

        switch (cv_mat.channels())
        {
        case 1:
            return Utils::cv2image(cv_mat, Image::Type::GRAYSCALE).clone();
        case 3:
            return Utils::cv2image(cv_mat, Image::Type::BGR).clone();
        default:
            throw std::runtime_error("Unsupported number of channels");
        }
    }

Loading the Deep Learning Model
-------------------------------

The deep learning models output by DaoAI World are usually in `dwm` format. We need to create a `DaoAI::DeepLearning::Vision::KeypointDetection` object and use the constructor method to load the deep learning model `dwm` file output by DaoAI World.

.. code-block:: C++

    // Model path in the server file system
    std::string model_path =  "C:/Users/daoai/test_vision/kp.dwm";

    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);

Note that each detection task has a corresponding object:

.. code-block:: C++

    // Instance segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

    // Keypoint detection
    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);
    
    // Image classification
    DaoAI::DeepLearning::Vision::Classification model(model_path);
    
    // Object detection
    DaoAI::DeepLearning::Vision::ObjectDetection model(model_path);
    
    // Anomaly detection
    DaoAI::DeepLearning::Vision::AnomalyDetection model(model_path);
    
    // Semantic segmentation
    DaoAI::DeepLearning::Vision::SemanticSegmentation model(model_path);
    
    // OCR
    DaoAI::DeepLearning::Vision::OCR model(model_path);

    // Positioning model (supported only in the industrial version)
    DaoAI::DeepLearning::Vision::Positioning model(model_path);

    // Presence checking (supported only in the industrial version)
    DaoAI::DeepLearning::Vision::PresenceChecking model(model_path);

If you try to load an incorrect model type, an error message will be thrown, indicating the correct model type to use.

Using the Deep Learning Model for Inference
-------------------------------------------

.. code-block:: C++

    // Get inference
    DaoAI::DeepLearning::Vision::KeypointDetectionResult result = model.inference(base64_encoded_image);

Note that each detection task has a corresponding result object:

.. code-block:: C++

    // Instance segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    // Keypoint detection
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    // Image classification
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    // Object detection
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    // Anomaly detection
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    // Semantic segmentation
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    // OCR
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

    // Positioning model (supported only in the industrial version)
    DaoAI::DeepLearning::Vision::PositioningResult prediction = model.inference(daoai_image);

    // Presence checking (supported only in the industrial version)
    DaoAI::DeepLearning::Vision::PresenceCheckingResult prediction = model.inference(daoai_image);

Example of Returned Results
---------------------------

Below is an example of the result returned by the keypoint detection model after prediction.

This result shows the label names, confidence, bounding box, keypoints, and polygon masks.

.. code-block:: C++

    std::cout << result.num_detections << "\n";
    for (int i = 0; i < result.num_detections; ++i)
    {
        std::cout << "Object " << std::to_string(i + 1) << "\n";
        std::cout << "Class: " << result.class_labels[i] << "\n";
        std::cout << "Bounding box: " << result.boxes[i].x1() << " " << result.boxes[i].y1() << " " << result.boxes[i].x2() << " " << result.boxes[i].x2() << "\n";
        std::cout << "Confidence: " << result.confidences[i] << "\n";
        std::cout << "Keypoints: \n";
        for (int j = 0; j < result.keypoints[i].size(); ++j)
        {
            std::cout << result.keypoints[i][j].x << " " << result.keypoints[i][j].y << " " << result.keypoints[i][j].confidence << "\n";
        }
        std::cout << "\n";
    }
