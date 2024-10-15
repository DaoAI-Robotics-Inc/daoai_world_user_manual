C++ 代码示例
===============

本章会详细介绍DaoAI World SDK中包含的C++代码示例。

引入库
--------------

在C++示例中，我们使用了以下几个头文件，其中 ``dlsdk/model.h`` 是用于引入DaoAI World SDK的库。

.. code-block:: C++

    #include <dlsdk/model.h>
    #include <dlsdk/prediction.h>
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

    //定位模型 (只在工业版支持)
    DaoAI::DeepLearning::Vision::Positioning model(model_path);

    //漏错装检测 (只在工业版支持)
    DaoAI::DeepLearning::Vision::PresenceChecking model(model_path);

如果尝试加载非对应的模型对象，那么会报错，报错信息中会提示您应该用的模型类型。

使用深度学习模型进行预测
--------------------------

.. code-block:: C++

		// get inference
		DaoAI::DeepLearning::Vision::ClassificationResult prediction = model.inference(daoai_image);

		//std::vector<DaoAI::DeepLearning::Polygon> polygons = prediction.masks[1].toPolygons();
		std::string json_string = prediction.toJSONString(); // 标准输出的Json
		std::string annotation_json_string = prediction.toAnnotationJSONString(); //按照数据的标注的格式输出Json 
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

    //定位模型 (只在工业版支持)
    DaoAI::DeepLearning::Vision::PositioningResult prediction = model.inference(daoai_image);

    //漏错装检测 (只在工业版支持)
    DaoAI::DeepLearning::Vision::PresenceCheckingResult prediction = model.inference(daoai_image);


返回结果示例
------------------

以下是实例分割模型预测后 调用 toJSONString() 方法所返回的结果示例。

这个结果展示了预测的数量，标签名称，置信度，以及预测框，和多边形掩膜。

.. code-block:: json

    {
        "Number of detecions": 1,
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


``DaoAI::DeepLearning::Vision::InstanceSegmentationResult`` 对象 还可以使用 .masks[i].toPolygons() 方法来获取多边形对象。

或者使用 .masks[i].toImage() 方法 来获取多边形掩膜的图像，您可以使用 openCV 来讲图片写出。

.. code-block:: C++

    DaoAI::DeepLearning::Image image = prediction.masks[0].toImage();
    cv::imwrite("mask.png", cv::Mat(image.rows,image.cols, CV_8UC1, image.getData()));

