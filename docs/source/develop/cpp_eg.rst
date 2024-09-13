C++ 代码示例
===============

本章会详细介绍DaoAI World SDK中包含的C++代码示例。

引入库
--------------

在C++示例中，我们使用了以下三个头文件，其中 ``dlsdk/model.h`` 是用于引入DaoAI World SDK的库。

.. code-block:: C++

    #include <dlsdk/model.h>
    #include <string>
    #include <fstream>


读取图片
-----------

DaoAI World SDK 的模型预测函数需要将图片表示为一维数组（1D array）。以下是从文件中读取图片的函数：

首先定义文件路径，然后调用函数读取。可以调用daoai_image()方法读取图片。

.. code-block:: C++

		// get root path to the model and image
        std::string root = "../"; // change to your own path
		std::string image_path = root + "image.png";

		// load image
		DaoAI::DeepLearning::Image daoai_image(image_path);


加载深度学习模型
-------------------

首先需要加载模型。DaoAI World 输出的深度学习模型通常是 zip 格式。我们需要创建一个 DaoAI::DeepLearning::Model 对象，然后调用 loadNestedZip 方法来读取 DaoAI World 输出的深度学习模型 zip 文件。

.. code-block:: C++

        std::string root = "../"; // change to your own path
		std::string model_path = root + "model.dwm";

        // init model
		DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

注意，这里每一个检测任务都有对应的对象：

.. code-block:: C++

    //实例分割
    DaoAI::DeepLearning::Vision::InstanceSegmentation model(model_path);

    //关键点检测
    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);
    
    //图像分类
    DaoAI::DeepLearning::Vision::Classification model(model_path);
    
    //目标检测
    DaoAI::DeepLearning::Vision::ObjectDetection model(model_path);
    
    //异常检测
    DaoAI::DeepLearning::Vision::AnomalyDetection model(model_path);
    
    //语义分割
    DaoAI::DeepLearning::Vision::SemanticSegmentation model(model_path);
    
    //OCR
    DaoAI::DeepLearning::Vision::OCR model(model_path);

如果尝试加载非对应的模型对象，那么会报错，报错信息中会提示您应该用的模型类型。

使用深度学习模型进行预测
--------------------------

.. code-block:: C++

		// get inference
		DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);

		//std::vector<DaoAI::DeepLearning::Polygon> polygons = prediction.masks[1].toPolygons();
		std::string json_string = prediction.toJSONString();
		// write to json file
		std::ofstream fout(root + "daoai_1.json");
		fout << json_string << "\n";
		fout.close();

注意，这里每一个检测任务返回的结果都有对应的对象：

.. code-block:: C++

    //实例分割
    DaoAI::DeepLearning::Vision::InstanceSegmentationResult prediction = model.inference(daoai_image);

    //关键点检测
    DaoAI::DeepLearning::Vision::KeypointDetectionResult prediction = model.inference(daoai_image);
    
    //图像分类
    DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);
    
    //目标检测
    DaoAI::DeepLearning::Vision::ObjectDetectionResult prediction = model.inference(daoai_image);
    
    //异常检测
    DaoAI::DeepLearning::Vision::AnomalyDetectionResult prediction = model.inference(daoai_image);
    
    //语义分割
    DaoAI::DeepLearning::Vision::SemanticSegmentationResult prediction = model.inference(daoai_image);
    
    //OCR
    DaoAI::DeepLearning::Vision::OCRResult prediction = model.inference(daoai_image);

返回结果示例
------------------

以下是实例分割模型预测后返回的结果示例。主要结果包含在 `shapes` 列表中。

这个结果展示了预测的多边形点（points）、标签（label）以及群组ID（group_id）。这些信息可以用来进一步处理或分析预测的结果。

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

``DaoAI::DeepLearning::Vision::InstanceSegmentationResult`` 对象 还可以使用 .masks[i].toPolygons() 方法来获取多边形对象。

或者使用 .masks[i].toImage() 方法 来获取多边形掩膜的图像，您可以使用 openCV 来讲图片写出。

.. code-block:: C++

    DaoAI::DeepLearning::Image image = prediction.maskss[0].toImage();
    cv::imwrite("mask.png", cv::Mat(image.rows,image.cols, CV_8UC1, image.getData()));

