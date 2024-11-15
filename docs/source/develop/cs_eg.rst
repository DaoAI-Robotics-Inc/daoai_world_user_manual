C# 代码示例
===============

本章会详细介绍DaoAI World SDK中包含的C#代码示例。

引入库
--------------

在C#示例中，我们引入了以下几个库，其中 ``DaoAI.DeepLearningCLI`` 是用于引入DaoAI World SDK的库。

.. code-block:: C#

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Drawing;
    using System.Threading.Tasks;
    using DaoAI.DeepLearningCLI;


读取图片
-----------

DaoAI World SDK 的模型预测函数需要将图片表示为一维数组（1D array）。以下是从文件中读取图片的代码部分：

.. code-block:: C#

    // Test image
    String root_directory = System.IO.Directory.GetCurrentDirectory();

    System.Drawing.Bitmap image = new
        System.Drawing.Bitmap("C:\\Users\\daoai\\Downloads\\DW_SDK\\DW_SDK Example\\Data\\maskrcnn_data\\daoai_1.png"); //图片文件路径
    System.Drawing.Bitmap image_copy = new System.Drawing.Bitmap(image);
    
    byte[] pixels = new byte[image.Width * image.Height * 3];
    for (int i = 0; i < image.Height; i++)
    {
        for (int j = 0; j < image.Width; j++)
        {
            System.Drawing.Color color = image.GetPixel(j, i);
            pixels[(i * image.Width + j) * 3] = (byte)(color.R);
            pixels[(i * image.Width + j) * 3 + 1] = (byte)(color.G);
            pixels[(i * image.Width + j) * 3 + 2] = (byte)(color.B);
        }
    }
    

这里做了一个图片的深度拷贝,然后用 `DaoAI.DeepLearningCLI.Image` 函数初始化图像对象以便后续使用：

.. code-block:: C#

    DaoAI.DeepLearningCLI.Image img = new DaoAI.DeepLearningCLI.Image(image.Height, image.Width, DaoAI.DeepLearningCLI.Image.Type.RGB, pixels);
    DaoAI.DeepLearningCLI.Image img_copy = img.clone();
    byte[] image_data = img_copy.data;

    for (int i = 0; i < image.Width; i++)
    {
        for (int j = 0; j < image.Height; j++)
        {
            int index_r = i * image.Height + j;
            int index_g = i * image.Height + j + image.Width * image.Height;
            int index_b = i * image.Height + j + 2 * image.Width * image.Height;
            byte r = image_data[index_r];
            byte g = image_data[index_g];
            byte b = image_data[index_b];
            System.Drawing.Color color = Color.FromArgb(r, g, b);
            image_copy.SetPixel(i, j, color);
        }
    }

加载深度学习模型
-------------------

.. code-block:: C#

        String data_path = "..\\..\\..\\..\\Data\\";
        String model_path = data_path + "model.dwm";
        // init model
        DaoAI.DeepLearningCLI.Vision.KeypointDetection model = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);


注意，这里每一个检测任务都有对应的对象：

.. code-block:: C#

    //实例分割
    DaoAI.DeepLearningCLI.Vision.InstanceSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.InstanceSegmentation(model_path);

    //关键点检测
    DaoAI.DeepLearningCLI.Vision.KeypointDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);
    
    //图像分类
    DaoAI.DeepLearningCLI.Vision.Classification model(model_path) = new DaoAI.DeepLearningCLI.Vision.Classification(model_path);
    
    //目标检测
    DaoAI.DeepLearningCLI.Vision.ObjectDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.ObjectDetection(model_path);

    //异常检测(适用于.6版本以前的类型名称，.7后更名为 非监督缺陷检测)
    DaoAI.DeepLearningCLI.Vision.AnomalyDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.AnomalyDetection(model_path);
    
    //语义分割(适用于.6版本以前的类型名称，.7后更名为 监督缺陷检测)
    DaoAI.DeepLearningCLI.Vision.SemanticSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.SemanticSegmentation(model_path);

    //非监督缺陷检测
    DaoAI.DeepLearningCLI.Vision.UnsupervisedDefectSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.AnomalyDetection(model_path);
    
    //监督缺陷检测
    DaoAI.DeepLearningCLI.Vision.SupervisedDefectSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.SemanticSegmentation(model_path);

    //OCR
    DaoAI.DeepLearningCLI.Vision.OCR model(model_path) = new DaoAI.DeepLearningCLI.Vision.OCR(model_path);

    //定位模型 (只在工业版支持)
    DaoAI.DeepLearningCLI.Vision.Positioning model(model_path) = new DaoAI.DeepLearningCLI.Vision.Positioning(model_path);

    //漏错装检测 (只在工业版支持)
    DaoAI.DeepLearningCLI.Vision.PresenceChecking model(model_path) = new DaoAI.DeepLearningCLI.Vision.PresenceChecking(model_path);


使用深度学习模型进行预测
--------------------------

这里定义了 置信度阈值(CONFIDENT_THRESHOLD)为 0.5, 并调用 model.inferece() 函数来使用模型进行推理，再使用 .toJSONString()方法 打印为 json

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENT_THRESHOLD] = 0.5;

    Console.WriteLine(model.inference(img, post_params).toJSONString());

后处理参数
~~~~~~~~~~~~~~

模型的预测 可以接受后处理参数：

    - DaoAI.DeepLearningCLI.PostProcessType.CONFIDENCE_THRESHOLD:
    
        置信度阈值，会过滤掉结果中置信度低于设定值的结果
    
    - post_params[DaoAI.DeepLearningCLI.PostProcessType.IOU_THRESHOLD:

        IOU阈值，会过滤掉结果中IOU低于设定值的结果
   
    - post_params[DaoAI.DeepLearningCLI.PostProcessType.SENSITIVITY_THRESHOLD:

        非监督缺陷分割（异常检测）模型中使用敏感度，控制模型对于缺陷的敏感度，越高则模型会检测出越多的缺陷，但是容易误检

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENCE_THRESHOLD] = 0.5; //置信度阈值，会过滤掉结果中置信度低于设定值的结果
    post_params[DaoAI.DeepLearningCLI.PostProcessType.IOU_THRESHOLD] = 0.5; //IOU阈值，会过滤掉结果中IOU低于设定值的结果

    Console.WriteLine(model.inference(img, post_params));


返回结果示例
------------------

以下是实例分割模型预测后返回的结果示例。

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
