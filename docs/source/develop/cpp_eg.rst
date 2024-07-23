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

首先定义文件路径，然后调用函数读取。

.. code-block:: C++

    std::string root = "../";
    std::string data_path = root + "Data/daoai_1.bin";

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
    // 这段代码演示了如何从二进制文件读取图像数据。你可以用自己的图像数据 (例如: OpenCV 库中data buffer函数)

    std::vector<uint8_t> readImageBinaryFile(const std::string& filePath) {
        std::ifstream inputFile(filePath, std::ios::binary);
        if (!inputFile) {
            std::cerr << "Cannot open " << filePath << "\n";
            return {};
        }

        std::vector<uint8_t> buffer((std::istreambuf_iterator<char>(inputFile)), std::istreambuf_iterator<char>());
        inputFile.close();

        return buffer;
    }

接下来，在 main 函数中，定义图片的尺寸，然后调用上述函数将图片读取到数组中，然后用daoai_image函数初始化 以便后续使用：

.. code-block:: C++

        const int image_height = 1200;
        const int image_width = 1920;

        std::vector<uint8_t> image_buffer = readImageBinaryFile(data_path);
        if (image_buffer.size() == 0)
        {
            std::cerr << "Image buffer is not read properly" << "\n";
            return 1;
        }

        DaoAI::DeepLearning::Image daoai_image(image_height, image_width, DaoAI::DeepLearning::Image::Type::BGR, &image_buffer[0]);


加载深度学习模型
-------------------

首先需要加载模型。DaoAI World 输出的深度学习模型通常是 zip 格式。我们需要创建一个 DaoAI::DeepLearning::Model 对象，然后调用 loadNestedZip 方法来读取 DaoAI World 输出的深度学习模型 zip 文件。

.. code-block:: C++

        std::string root = "../";
        std::string model_zip_path = root + "Data/test_kp_fast.zip";

        // init model
        DaoAI::DeepLearning::Model model;
        
        std::cout << "Loading Model" << "\n";
        // load model
        model.load(model_zip_path);
        std::cout << "Model Loaded" << "\n";



使用深度学习模型进行预测
--------------------------

调用 `DaoAI::DeepLearning::Model` 的 `inferenceJSON()` 方法可以对一个图像对象进行深度学习预测。该方法会返回一组 JSON 格式的预测结果。

在不需要模型的时候，记得调用 `unload()` 方法释放内存。

.. code-block:: C++

		// get inference
		auto prediction = model.inferenceJSON(daoai_image);

		// write to json file
		std::ofstream fout(root + "Data/daoai_1.json");
		fout << prediction << "\n";
		fout.close();

		// unload model
		model.unload();


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

