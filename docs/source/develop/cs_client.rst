C# Inference Client 示例项目
====================================

.. contents::
    :local:
    
本章会详细介绍DaoAI World SDK中包含的C# Inference Client 代码示例。

引入库
--------------

在C#示例中，我们使用了以下几个库

.. code-block:: C#

    using System;
    using System.IO;
    using DaoAI.InferenceClient;

读取图片
-----------

C# Inference Client 的模型预测函数需要将图片表示为64位编码图片（base64_encoded_image） 您可以使用Convert.ToBase64String() 方法来转换。


首先定义文件路径 文件的格式是转换成64位图片后的txt文件，然后读取。

.. code-block:: C#

    string fileName = "C:/Users/daoai/Documents/DWSDK_Demo/data/image.bmp";

    string base64Image = Convert.ToBase64String(File.ReadAllBytes(fileName));


加载深度学习模型
-------------------

DaoAI World 输出的深度学习模型通常是 dwm 格式。我们需要创建一个 DaoAI.InferenceClient.KeypointDetection 对象，然后使用constructor 方法来读取 DaoAI World 输出的深度学习模型 dwm 文件。

.. code-block:: C#

    // model path in server file system
    string filemodel = "../../../../../../data/KeypointDetection.dwm";

    DaoAI.InferenceClient.KeypointDetection model = new DaoAI.InferenceClient.KeypointDetection(filePath_model, DaoAI.InferenceClient.DeviceType.GPU);


注意，这里每一个检测任务都有对应的对象：

.. code-block:: C#

    //实例分割
    DaoAI.InferenceClient.InstanceSegmentation model(model_path);

    //关键点检测
    DaoAI.InferenceClient.KeypointDetection model(model_path);
    
    //图像分类
    DaoAI.InferenceClient.Classification model(model_path);
    
    //目标检测
    DaoAI.InferenceClient.ObjectDetection model(model_path);

    //非监督缺陷检测
    DaoAI.InferenceClient.UnsupervisedDefectSegmentation model(model_path);
    
    //监督缺陷检测
    DaoAI.InferenceClient.SupervisedDefectSegmentation model(model_path);
    
    //OCR
    DaoAI.InferenceClient.OCR model(model_path);

    //定位模型 (只在工业版支持)
    DaoAI.InferenceClient.Positioning model(model_path);

    //漏错装检测 (只在工业版支持)
    DaoAI.InferenceClient.PresenceChecking model(model_path);

如果尝试加载非对应的模型对象，那么会报错，报错信息中会提示您应该用的模型类型。

使用深度学习模型进行预测
--------------------------

.. code-block:: C#

    // get inference
    DaoAI.InferenceClient.KeypointDetection prediction = model.inference(base64Image);
            
注意，这里每一个检测任务返回的结果都有对应的对象：

.. code-block:: C#

    //实例分割
    DaoAI.InferenceClient.InstanceSegmentationResult prediction = model.inference(base64Image);

    //关键点检测
    DaoAI.InferenceClient.KeypointDetectionResult prediction = model.inference(base64Image);
    
    //图像分类
    DaoAI.InferenceClient.ClassificationResult prediction = model.inference(base64Image);
    
    //目标检测
    DaoAI.InferenceClient.ObjectDetectionResult prediction = model.inference(base64Image);
    
    //异常检测
    DaoAI.InferenceClient.AnomalyDetectionResult prediction = model.inference(base64Image);
    
    //语义分割
    DaoAI.InferenceClient.SemanticSegmentationResult prediction = model.inference(base64Image);
    
    //OCR
    DaoAI.InferenceClient.OCRResult prediction = model.inference(base64Image);

    //定位模型 (只在工业版支持)
    DaoAI.InferenceClient.PositioningResult prediction = model.inference(base64Image);

    //漏错装检测 (只在工业版支持)
    DaoAI.InferenceClient.PresenceCheckingResult prediction = model.inference(base64Image);


返回结果示例
------------------

以下是关键点检测模型预测后返回的结果.

这个结果展示了，标签名称，置信度，以及预测框，关键点和多边形掩膜。

.. code-block:: C#

    for (int i = 0; i < result.NumDetections; i++)
    {
        Console.WriteLine($"Object {i + 1}");
        Console.WriteLine($"Class: {result.ClassLabels[i]}");
        Console.WriteLine($"Bounding box: {result.Boxes[i].X1} {result.Boxes[i].Y1} {result.Boxes[i].X2} {result.Boxes[i].Y2}");
        Console.WriteLine($"Confidence: {result.Confidences[i]}");
        Console.WriteLine("Keypoints:");

        foreach (var keypoint in result.Keypoints[i])
        {
            Console.WriteLine($"{keypoint.X} {keypoint.Y} {keypoint.Confidence}");
        }
        Console.WriteLine();
    }