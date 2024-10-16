C++ Inference Client 示例项目
====================================


本章会详细介绍DaoAI World SDK中包含的C++ Inference Client 代码示例。

引入库
--------------

在C++示例中，我们使用了以下几个头文件。

.. code-block:: C++

    #include <iostream>
    #include <inference_client/model.h>
    #include <inference_client/common.h>
    #include <string>
    #include <fstream>


读取图片
-----------

C++ Inference Client 的模型预测函数需要将图片表示为64位编码图片（base64_encoded_image） 您可以使用Opencv库的函数来转换，或者使用网页工具 https://base64.guru/converter/encode/image 来转换。


首先定义文件路径 文件的格式是转换成64位图片后的txt文件，然后读取。

.. code-block:: C++

	// image path in local file system
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


您也可以使用Opencv库来读取图片为 64位编码格式。

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

加载深度学习模型
-------------------

DaoAI World 输出的深度学习模型通常是 dwm 格式。我们需要创建一个 DaoAI::DeepLearning::Vision::KeypointDetection 对象，然后使用constructor 方法来读取 DaoAI World 输出的深度学习模型 dwm 文件。

.. code-block:: C++

    // model path in server file system
    std::string model_path =  "C:/Users/daoai/test_vision/kp.dwm";

    DaoAI::DeepLearning::Vision::KeypointDetection model(model_path);

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
    DaoAI::DeepLearning::Vision::KeypointDetectionResult result = model.inference(base64_encoded_image);

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

以下是关键点检测模型预测后返回的结果.

这个结果展示了，标签名称，置信度，以及预测框，关键点和多边形掩膜。

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
