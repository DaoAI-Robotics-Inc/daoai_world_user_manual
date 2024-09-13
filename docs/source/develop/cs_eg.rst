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
        System.Drawing.Bitmap("C:\\Users\\daoai\\Downloads\\DLSDK\\DLSDK Example\\Data\\maskrcnn_data\\daoai_1.png"); //图片文件路径
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
    
    //异常检测
    DaoAI.DeepLearningCLI.Vision.AnomalyDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.AnomalyDetection(model_path);
    
    //语义分割
    DaoAI.DeepLearningCLI.Vision.SemanticSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.SemanticSegmentation(model_path);
    
    //OCR
    DaoAI.DeepLearningCLI.Vision.OCR model(model_path) = new DaoAI.DeepLearningCLI.Vision.OCR(model_path);


使用深度学习模型进行预测
--------------------------

这里定义了 置信度阈值(CONFIDENT_THRESHOLD)为 0.5, 并调用 model.inferece() 函数来使用模型进行推理，再使用 .toJSONString()方法 打印为 json

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENT_THRESHOLD] = 0.5;

    Console.WriteLine(model.inference(img, post_params).toJSONString());

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

