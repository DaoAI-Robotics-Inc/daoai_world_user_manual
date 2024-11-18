C++ Inference Client Example Project
====================================

This chapter provides a detailed introduction to the C++ Inference Client code examples included in the DaoAI World SDK.

Importing Libraries
--------------------

In the C++ examples, the following headers are used:

.. code-block:: cpp

    #include <iostream>
    #include <inference_client/model.h>
    #include <inference_client/common.h>
    #include <string>
    #include <fstream>


Reading Images
--------------

The C++ Inference Client's model inference function requires images to be represented as a 64-bit encoded image (base64_encoded_image). You can use OpenCV library functions for conversion or an online tool like `Base64 Guru <https://base64.guru/converter/encode/image>`_.

First, define the file path. The file format is a text file containing the 64-bit encoded image, which can then be read as follows:

.. code-block:: cpp

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

You can also use the OpenCV library to read and encode images into base64 format:

.. code-block:: cpp

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


Loading a Deep Learning Model
------------------------------

DaoAI World models are typically in the ``.dwm`` format. Create a ``DaoAI::DeepLearning::Vision::KeypointDetection`` object and use the constructor method to load the model file.

.. code-block:: cpp

    // Model path in the local file system
    std::string model_path = "C:/Users/daoai/test_vision/kp.dwm";

    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);

Each detection task corresponds to a specific model object:

.. code-block:: cpp

    // Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

    // Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);

    // Image Classification
    DaoAI::DeepLearning::Vision::Classification model(model_path);

    // Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetection model(model_path);

    // Anomaly Detection (prior to version 0.7)
    DaoAI::DeepLearning::Vision::AnomalyDetection model(model_path);

    // Semantic Segmentation (prior to version 0.7)
    DaoAI::DeepLearning::Vision::SemanticSegmentation model(model_path);

    // Unsupervised Defect Detection
    DaoAI::DeepLearning::Vision::UnsupervisedDefectSegmentation model(model_path);

    // Supervised Defect Detection
    DaoAI::DeepLearning::Vision::SupervisedDefectSegmentation model(model_path);

    // OCR
    DaoAI::DeepLearning::Vision::OCR model(model_path);

    // Positioning Model (Industrial version only)
    DaoAI::DeepLearning::Vision::Positioning model(model_path);

    // Presence Checking (Industrial version only)
    DaoAI::DeepLearning::Vision::PresenceChecking model(model_path);

Loading an incorrect model type will throw an error, indicating the correct model object to use.

Performing Inference
---------------------

.. code-block:: cpp

    // Get inference results
    DaoAI::DeepLearning::Vision::KeypointDetectionResult result = model.inference(base64_encoded_image);

Each detection task returns a specific result object:

.. code-block:: cpp

    // Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    // Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    // Image Classification
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    // Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    // Anomaly Detection (used for version .6 and earlier; renamed to Unsupervised Defect Detection after version .7)
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    // Semantic Segmentation (used for version .6 and earlier; renamed to Supervised Defect Detection after version .7)
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    // Unsupervised Defect Detection
    DaoAI::DeepLearning::Vision::UnsupervisedDefectSegmentationResult prediction = model.inference(daoai_image);
    
    // Supervised Defect Detection
    DaoAI::DeepLearning::Vision::SupervisedDefectSegmentationResult prediction = model.inference(daoai_image);
    
    // OCR (Optical Character Recognition)
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

    // Positioning Model (available only in the industrial version)
    DaoAI::DeepLearning::Vision::PositioningResult prediction = model.inference(daoai_image);

    // Presence Checking (available only in the industrial version)
    DaoAI::DeepLearning::Vision::PresenceCheckingResult prediction = model.inference(daoai_image);


Example Inference Result
-------------------------

The following is an example result from a Keypoint Detection model inference. It displays the label name, confidence, bounding box, keypoints, and polygon masks.

.. code-block:: cpp

    std::cout << result.num_detections << "\n";
    for (int i = 0; i < result.num_detections; ++i)
    {
        std::cout << "Object " << std::to_string(i + 1) << "\n";
        std::cout << "Class: " << result.class_labels[i] << "\n";
        std::cout << "Bounding box: " << result.boxes[i].x1() << " " << result.boxes[i].y1() << " "
                  << result.boxes[i].x2() << " " << result.boxes[i].y2() << "\n";
        std::cout << "Confidence: " << result.confidences[i] << "\n";
        std::cout << "Keypoints: \n";
        for (int j = 0; j < result.keypoints[i].size(); ++j)
        {
            std::cout << result.keypoints[i][j].x << " " << result.keypoints[i][j].y << " "
                      << result.keypoints[i][j].confidence << "\n";
        }
        std::cout << "\n";
    }