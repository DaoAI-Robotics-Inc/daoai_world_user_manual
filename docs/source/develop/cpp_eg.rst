C++ Code Example
===================

This chapter provides detailed explanations of the C++ code examples included in the DaoAI World SDK.

Importing Libraries
------------------------

In the C++ examples, we use the following headers, with ``dlsdk/model.h`` being used to include the DaoAI World SDK library.

.. code-block:: cpp

    #include <dlsdk/model.h>
    #include <dlsdk/prediction.h>
    #include <string>
    #include <fstream>

Reading Images
-----------------

The model prediction function in DaoAI World SDK requires the image to be represented as a one-dimensional array (1D array). Below is the function to read an image from a file:

First, define the file path and then call the function to read it. You can use the ``daoai_image()`` method to load the image.

.. code-block:: cpp

    // get root path to the model and image
    std::string root = "../"; // change to your own path
    std::string image_path = root + "image.png";

    // load image
    DaoAI::DeepLearning::Image daoai_image(image_path);

Loading a Deep Learning Model
----------------------------------

First, you need to load the model. The deep learning model output by DaoAI World is typically in `.dwm` format. We need to create a ``DaoAI::DeepLearning::Vision::InstanceSegmentation`` object and use the constructor method to load the `.dwm` file output by DaoAI World.

.. code-block:: cpp

    std::string root = "../"; // change to your own path
    std::string model_path = root + "model.dwm";

    // init model
    DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

Note that each detection task has a corresponding object:

.. code-block:: cpp

    // Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

    // Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);

    // Image Classification
    DaoAI::DeepLearning::Vision::Classification model(model_path);

    // Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetection model(model_path);

    // Anomaly Detection (used for version .6 and earlier, renamed to Unsupervised Defect Detection after version .7)
    DaoAI::DeepLearning::Vision::AnomalyDetection model(model_path);

    // Semantic Segmentation (used for version .6 and earlier, renamed to Supervised Defect Detection after version .7)
    DaoAI::DeepLearning::Vision::SemanticSegmentation model(model_path);

    // Unsupervised Defect Detection
    DaoAI::DeepLearning::Vision::UnsupervisedDefectSegmentation model(model_path);

    // Supervised Defect Detection
    DaoAI::DeepLearning::Vision::SupervisedDefectSegmentation model(model_path);

    // OCR
    DaoAI::DeepLearning::Vision::OCR model(model_path);

    // Positioning Model (available only in the industrial version)
    DaoAI::DeepLearning::Vision::Positioning model(model_path);

    // Presence Checking (available only in the industrial version)
    DaoAI::DeepLearning::Vision::PresenceChecking model(model_path);

If you attempt to load a non-corresponding model object, an error will be thrown with a message indicating the correct model type.

Using Deep Learning Models for Prediction
---------------------------------------------

.. code-block:: cpp

    // get inference
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    //std::vector<DaoAI::DeepLearning::Polygon> polygons = prediction.masks[1].toPolygons();
    std::string json_string = prediction.toJSONString(); // Standard output JSON
    std::string annotation_json_string = prediction.toAnnotationJSONString(); // Output JSON in annotated format
    // write to json file
    std::ofstream fout(root + "daoai_1.json");
    fout << json_string << "\n";
    fout.close();

Note that each detection task returns a corresponding result object:

.. code-block:: cpp

    // Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    // Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    // Image Classification
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    // Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    // Anomaly Detection (used for version .6 and earlier, renamed to Unsupervised Defect Detection)
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    // Semantic Segmentation (used for version .6 and earlier, renamed to Supervised Defect Detection)
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    // Unsupervised Defect Detection
    DaoAI::DeepLearning::Vision::UnsupervisedDefectSegmentationResult prediction = model.inference(daoai_image);
    
    // Supervised Defect Detection
    DaoAI::DeepLearning::Vision::SupervisedDefectSegmentationResult prediction = model.inference(daoai_image);
    
    // OCR
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

    // Positioning Model (available only in the industrial version)
    DaoAI::DeepLearning::Vision::PositioningResult prediction = model.inference(daoai_image);

    // Presence Checking (available only in the industrial version)
    DaoAI::DeepLearning::Vision::PresenceCheckingResult prediction = model.inference(daoai_image);

Post-processing Parameters
---------------------------

The model prediction function can accept post-processing parameters:

    - ``DaoAI::DeepLearning::PostProcessType::CONFIDENCE_THRESHOLD``:

        The confidence threshold, which filters out results with a confidence lower than the set value.

    - ``DaoAI::DeepLearning::PostProcessType::IOU_THRESHOLD``:

        The IOU threshold, which filters out results with an IOU lower than the set value.

    - ``DaoAI::DeepLearning::PostProcessType::SENSITIVITY_THRESHOLD``:

        The sensitivity threshold, used in unsupervised defect segmentation (anomaly detection) models to control the sensitivity to defects. A higher sensitivity will result in more defects being detected, but may also lead to false positives.

.. code-block:: cpp

    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image, {{DaoAI::DeepLearning::PostProcessType::CONFIDENCE_THRESHOLD, 0.4}, {DaoAI::DeepLearning::PostProcessType::IOU_THRESHOLD, 0.5} });

Example of Return Results
--------------------------

Below is an example of the result returned by the ``toJSONString()`` method after prediction with the instance segmentation model.

This result shows the number of predictions, label names, confidence, bounding boxes, and polygon masks.

.. code-block:: json

    {
        "Number of detections": 1,
        "Detections": [
            {
                "Label": "zheng",
                "Confidence": 0.9523001313209534,
                "Box": [
                    955.1925659179688, 316.0162048339844, 1064.072021484375,
                    426.4408264160156
                ],
                "Mask": [
                    [990.0, 316.0],
                    [988.0, 318.0],
                    [987.0, 318.0],
                    [985.0, 320.0],
                    [982.0, 320.0],
                    [980.0, 322.0],
                    [979.0, 322.0],
                    [974.0, 327.0],
                    [972.0, 327.0],
                    [972.0, 328.0],
                    [1040.0, 316.0]
                ]
            }
        ],
        "ImageHeight": 1200,
        "ImageWidth": 1920
    }

The ``DaoAI::DeepLearning::Vision::InstanceSegmentationResult`` object can also use the ``.masks[i].toPolygons()`` method to get the polygon object.

Alternatively, use the ``.masks[i].toImage()`` method to get the polygon mask image, which you can then write using OpenCV.

.. code-block:: cpp

    DaoAI::DeepLearning::Image image = prediction.masks[0].toImage();
    cv::imwrite("mask.png", cv::Mat(image.rows, image.cols, CV_8UC1, image.getData()));
