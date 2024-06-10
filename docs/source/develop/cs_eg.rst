C++C# 代码示例
===============

本章会详细介绍DaoAI World SDK中包含的C#代码示例。

引入库
--------------

在C#示例中，我们引入了以下几个库，其中 ``DaoAI.DeepLearningCLI`` 是用于引入DaoAI World SDK的库。

.. code-block:: C++

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

.. code-block:: C++

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

.. code-block:: C++

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

首先需要加载模型。DaoAI World 输出的深度学习模型通常是 zip 格式。我们需要创建一个 `DaoAI.DeepLearningCLI.Model` 对象，然后调用 `loadNestedZip` 方法来读取 DaoAI World 输出的深度学习模型 zip 文件。

.. code-block:: C++

        DaoAI.DeepLearningCLI.Application.initialize();
        DaoAI.DeepLearningCLI.Model model = new DaoAI.DeepLearningCLI.Model();
        String data_path = "C:\\Users\\daoai\\Downloads\\DLSDK\\DLSDK Example\\Data\\";
        String model_path = data_path + "test_sem3.zip";
        // init model
        Console.WriteLine(DaoAI.DeepLearningCLI.Application.checkDaoAIModelValidity(model_path));
        // load model
        model.loadNestedZip(model_path, DaoAI.DeepLearningCLI.Device_Type.GPU, -1);


使用深度学习模型进行预测
--------------------------

调用 `DaoAI.DeepLearningCLI.Model` 的 `inferenceJSON()` 方法可以对一个图像对象进行深度学习预测。该方法会返回一组 JSON 格式的预测结果。

.. code-block:: C++

		Console.WriteLine(model.inferenceJson(img));

返回结果示例
------------------

以下是模型预测后返回的结果示例。主要结果包含在 `shapes` 列表中。

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

