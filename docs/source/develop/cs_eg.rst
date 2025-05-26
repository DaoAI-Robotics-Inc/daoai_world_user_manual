C# 代码示例
===============

本章会详细介绍DaoAI World SDK中包含的C#代码示例。

.. contents::
    :local:

您也可以查看我们的 GitHub repo，其中包含 C++、C# 和 Python 的示例项目，方便用户快速上手和参考。

链接： `DaoAI World SDK Desktop Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_

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

设置环境变量
--------------

通常来说，您在使用DW SDK的时候，系统环境变量是已经配置好的。但在少部分情况下，您可能会有包含冲突的动态链接库的路径。

您可以在不更改系统环境变量的前提下正常使用DW SDK。 只需要在代码的开始设置环境变量即可。

.. code-block:: C#

    Environment.SetEnvironmentVariable(
        "PATH",
        Environment.GetEnvironmentVariable("DWSDK_PATH") + @"\bin;" +
        Environment.GetEnvironmentVariable("DWSDK_PATH") + @"\3rd_party;" +
        Environment.GetEnvironmentVariable("PATH"),
        EnvironmentVariableTarget.Process
    );


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

.. note::
    
    - 在第一次运行时，需要加载模型等数据到内存，这通常需要更久的时间。在程序的后续运行中，也就是从第二次图片加载，推理时，运行时间就会稳定在更快速的时间。
    
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

    //旋转目标检测
    DaoAI.DeepLearningCLI.Vision.RotatedObjectDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.RotatedObjectDetection(model_path);

    //混合模型
    DaoAI.DeepLearningCLI.Vision.MultilabelDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.MultilabelDetection(model_path);

    //非监督缺陷检测 参考非监督示例代码
    
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

这里定义了 置信度阈值(setConfidenceThreshold)为 0.5, 并调用 model.inferece() 函数来使用模型进行推理，再使用 .toJSONString()方法 打印为 json

.. code-block:: C#

    model.setConfidenceThreshold(0.5f);

    Console.WriteLine(model.inference(img).toJSONString());

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

获取预测结果
------------------

Box（边界框）
~~~~~~~~~~~~~~~~~~~~~

Box 是模型预测结果中的边界框，可通过以下方式获取：

.. code-block:: C#

    // 获取预测结果
    DaoAI.DeepLearningCLI.Vision.KeypointDetectionResult prediction = model.Inference(daoaiImage);

    // 访问边界框坐标
    double x1 = prediction.boxes[0].x1();  // 左上角 x 坐标
    double y1 = prediction.boxes[0].y1();  // 左上角 y 坐标
    double x2 = prediction.boxes[0].x2();  // 右下角 x 坐标
    double y2 = prediction.boxes[0].y2();  // 右下角 y 坐标

Mask（掩码）
~~~~~~~~~~~~~~~~

Mask 是预测结果中目标物体的外轮廓信息，可以通过以下方式提取多边形区域的顶点：

.. code-block:: C#

    // 获取预测结果
    DaoAI.DeepLearningCLI.Vision.KeypointDetectionResult prediction = model.Inference(daoaiImage);

    // 获取多边形区域的第一个顶点
    double x = prediction.masks[0].toPolygons()[0].points[0].x; // 顶点的 X 坐标
    double y = prediction.masks[0].toPolygons()[0].points[0].y; // 顶点的 Y 坐标

- **`masks[0]`**：提取第一个目标物体的掩码。  
- **`toPolygons()`**：将掩码转换为多边形外轮廓。  
- **`points[0]`**：获取多边形的第一个顶点坐标。  

通过顺序连接多边形的所有顶点，可以形成完整的轮廓区域，用于目标检测、区域分析等应用场景。
所有顶点按顺序连接，形成目标物体的完整多边形区域。此方法适合需要详细外轮廓信息的场景，如区域分析或精细标注。

可视化输出
~~~~~~~~~~~~

生成并保存预测结果与原图叠加的可视化图像：

.. code-block:: C#

    DaoAI.DeepLearningCLI.Image result = DaoAI.DeepLearningCLI.Utils.visualize(img, prediction);
    result.save(root_directory + "daoai_output.png");

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
