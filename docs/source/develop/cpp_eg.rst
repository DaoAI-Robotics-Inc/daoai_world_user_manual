C++ Code Examples
=========================

This chapter will provide detailed examples of C++ code included in the DaoAI World SDK.


Including Libraries
--------------------------

In the C++ example, we use the following three header files, with ``dlsdk/model.h`` being used to include the DaoAI World SDK library.

.. code-block:: C++

    #include <dlsdk/model.h>
    #include <string>
    #include <fstream>


Reading Images
---------------------

The model prediction function in the DaoAI World SDK requires the image to be represented as a one-dimensional array (1D array). Below is a function to read an image from a file:

First, define the file path, and then call the function to read the image. You can use the daoai_image() method to read the image.

.. code-block:: C++

		// get root path to the model and image
        std::string root = "../"; // change to your own path
		std::string image_path = root + "image.png";

		// load image
		DaoAI::DeepLearning::Image daoai_image(image_path);


Loading Deep Learning Models
-------------------------------------

First, you need to load the model. The deep learning models output by DaoAI World are typically in zip format. 
You need to create a ``DaoAI::DeepLearning::Model`` object to read the deep learning model zip file output by DaoAI World.

.. code-block:: C++

        std::string root = "../"; // change to your own path
		std::string model_path = root + "model.dwm";

        // init model
		DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

Note that each detection task has a corresponding object:


.. code-block:: C++

    //Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    //Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    //Image Classification
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    //Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    //Unsupervised Defect Segmentation
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    //Supervised Defect Segmentation
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    //OCR
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

If you attempt to load a model object that does not match, an error will be reported, indicating the correct model type you should use.

Using Deep Learning Models for Prediction
--------------------------------------------------------

.. code-block:: C++

		// get inference
		DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);

		//std::vector<DaoAI::DeepLearning::Polygon> polygons = prediction.masks[1].toPolygons();
		std::string json_string = prediction.toJSONString();
		// write to json file
		std::ofstream fout(root + "daoai_1.json");
		fout << json_string << "\n";
		fout.close();

Note that each detection task returns results with corresponding objects:


.. code-block:: C++

    //Instance Segmentation
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    //Keypoint Detection
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    //Image Classification
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    //Object Detection
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    //Unsupervised Defect Segmentation
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    //Supervised Defect Segmentation
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    //OCR
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

Example of Returned Results
------------------------------

Below is an example of the results returned by the instance segmentation model. The main results are included in the ``shapes`` list.

This result shows the predicted polygon points, labels, and group IDs. This information can be used for further processing or analysis of the prediction results.

.. code-block:: json

    {
    "flags": {},
    "shapes": [
        {
        "label": "back",
        "points": [
            [1525.5, 928.5],
            [1522.5, 931.5],
            [1528.5, 931.5],
            [1527.0, 930.0],
            [1527.0, 928.5]
        ],
        "group_id": 1,
        "description": "",
        "shape_type": "polygon",
        "flags": {}
        },
        {
        "label": "front",
        "points": [
            [1428.0, 798.0],
            [1429.5, 796.5],
            [1431.0, 796.5],
            [1432.5, 798.0],
            [1432.5, 801.0],
            [1431.0, 802.5],
            [1425.0, 802.5],
            [1423.5, 801.0],
            [1426.5, 798.0]
        ],
        "group_id": 0,
        "description": "",
        "shape_type": "polygon",
        "flags": {}
        },
    ],
    "imageWidth": 1920,
    "imageHeight": 1200
    }

The ``DaoAI::DeepLearning::Vision::InstanceSegmentationResult`` object also provides the .masks[i].toPolygons() method to obtain polygon objects.

Alternatively, you can use the ``.masks[i].toImage()`` method to get the image of the polygon mask, which you can then write out using OpenCV.

.. code-block:: C++

    DaoAI::DeepLearning::Image image = prediction.maskss[0].toImage();
    cv::imwrite("mask.png", cv::Mat(image.rows,image.cols, CV_8UC1, image.getData()));

