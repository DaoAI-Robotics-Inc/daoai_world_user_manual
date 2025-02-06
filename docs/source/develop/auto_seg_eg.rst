
C++ 智能分割示例项目
=============================

概述
------

本软件提供了一个交互式图像查看器，允许用户在图像上绘制边界框并点击点来执行基于深度学习模型的自动分割。分割结果会作为 JSON 文件保存，并在窗口中显示应用了掩码的图像。

该程序使用了 `DaoAI AutoSegmentation` 模型，基于用户绘制的边界框和点击的点进行推理。分割结果与原始图像混合，提供掩码的视觉表示。

智能分割功能 属于常规本地SDK的功能，同样也支持 **C#** 和 **Python**

您也可以查看我们的 GitHub repo，其中包含 C++、C# 和 Python 的示例项目，方便用户快速上手和参考。

链接： `DaoAI World SDK Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_


前置条件
----------
- **OpenCV 库 v15**：用于显示和与图像进行交互。
- **安装DaoAI DWSDK**：用于利用 `DaoAI AutoSegmentation` 模型。
- **深度学习模型文件** ( `auto_segment.dwm <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EcAQRAxfS6tGqmz21K3SJGIBD27g-aaf5RWvF7Z1MO6v8w?e=R6TGJq>`_ ) 
- **待分割的图像文件**
- **下载示例项目**  `Auto Segment C++ 示例项目 <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/Ed_wyHsxcu9Li_PoXNlsFLsBMFMdycuu4QuE_bIghj-Lrw?e=VXllfb>`_  

安装
----

1. 确保已安装必要的依赖项：
   - OpenCV v15
   - DaoAI 深度学习 SDK

2. 下载深度学习模型：
   - 模型文件应为 `auto_segment.dwm <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EcAQRAxfS6tGqmz21K3SJGIBD27g-aaf5RWvF7Z1MO6v8w?e=R6TGJq>`_ 

3. 准备用于分割的图像：
   - 图像文件应为 OpenCV 支持的标准格式，如 `.png`、 `.jpg` 或 `.bmp` 。

使用方法
----------

1. **运行程序**：

   - 下载示例项目，解压后，打开 DLSDK Example.sln, 右键项目 -> 项目属性， 在 C++ 和 Linker 页 配置 OpenCV 库 v15 的路径。

.. note::
    
    - 在第一次运行时 需要加载模型等数据。通常需要更久的时间，在程序的后续运行中 也就是从第二次图片加载时，这部分的加载时间约为 0.2秒
    - 内存占用约为 1G

2. **与图像交互**：
   - **左键拖拽**：点击并拖动鼠标以绘制边界框。分割出框内的物体。
   - **左键点击**：在图像上点击选择分割点。每次左键点击都会添加一个点，代表分割的图像希望包含这个点。
   - **右键点击**：在图像上点击选择分割点。每次右键点击都会添加一个点，代表分割的图像不希望包含这个点。

3. **执行分割**：
   - 当松开鼠标按钮后，程序会根据边界框和选择的点触发推理。
   - 结果将通过 `AutoSegmentation` 模型进行处理，返回分割掩码。

4. **保存结果**：
   - 分割结果以及推理细节会保存在 result.json 文件中，文件位于与输入图像相同的目录。
   - JSON 文件包含分割掩码和其他信息。

5. **重置图像**：
   - 按 **'r'** 键可清除边界框和点击的点，重置为原始图像。

6. **退出程序**：
   - 按 **'Esc'** 键退出程序。


示例代码
-----------------

.. code-block:: C++

    #include <opencv2/opencv.hpp>
    #include <iostream>
    #include <vector>
    #include <fstream>
    #include <dlsdk/model.h>

    using namespace DaoAI::DeepLearning;

    // Global variables
    std::vector<Point> clicked_points;
    std::vector<Box> drawn_boxes;
    bool is_drawing = false;
    Point start_point;
    cv::Mat* original_image = nullptr;
    Vision::AutoSegmentation* model = nullptr;
    Vision::ImageEmbedding* embedding = nullptr;
    const std::string window_name = "Image Viewer";
    const int drag_threshold = 5;

    // Save JSON result to a file
    void saveResultToFile(const std::string& json_string, const std::string& image_path) {
        size_t last_slash_idx = image_path.find_last_of("/\\");
        std::string directory = (last_slash_idx == std::string::npos) ? "" : image_path.substr(0, last_slash_idx + 1);
        std::string output_path = directory + "result.json";

        std::ofstream file(output_path);
        if (file.is_open()) {
            file << json_string;
            file.close();
            std::cout << "Result saved to: " << output_path << std::endl;
        } else {
            std::cerr << "Error: Could not save result to " << output_path << std::endl;
        }
    }

    // Mouse callback function
    void onMouse(int event, int x, int y, int flags, void* userdata) {
        static bool is_click_detected = false; // Track single clicks
        cv::Mat display_image = original_image->clone();

        if (event == cv::EVENT_LBUTTONDOWN) {
            is_drawing = true;
            is_click_detected = true; // Assume it's a click unless a drag is detected
            start_point = Point(x, y);
        } else if (event == cv::EVENT_MOUSEMOVE && is_drawing) {
            if (std::abs(x - start_point.x) > drag_threshold || std::abs(y - start_point.y) > drag_threshold) {
                is_click_detected = false; // It's a drag
                cv::rectangle(display_image, cv::Point(start_point.x, start_point.y), cv::Point(x, y), cv::Scalar(0, 255, 0), 2);
                cv::imshow(window_name, display_image);
            }
        } else if (event == cv::EVENT_LBUTTONUP) {
            is_drawing = false;
            Point end_point(x, y);

            if (is_click_detected) {
                clicked_points.push_back(Point(x, y, "1"));
            } else {
                drawn_boxes.push_back(Box(start_point, end_point));
                cv::rectangle(display_image, cv::Point(start_point.x, start_point.y), cv::Point(end_point.x, end_point.y), cv::Scalar(0, 255, 0), 2);
            }

            // Perform inference
            auto result = model->inference(*embedding, drawn_boxes, clicked_points);
            auto daoai_mask_image = result.mask.toImage();

            // Save result to file
            saveResultToFile(result.toJSONString(), *(std::string*)userdata);

            // Convert the mask to OpenCV format
            cv::Mat mask_image(daoai_mask_image.height, daoai_mask_image.width, CV_8UC1, daoai_mask_image.getData());
            mask_image = mask_image.clone();

            // Create a masked image
            cv::Mat masked_image;
            original_image->copyTo(masked_image, mask_image);

            // Blend the original and masked images
            cv::Mat blended_image;
            cv::addWeighted(*original_image, 0.3, masked_image, 0.7, 0, blended_image);

            // Display the blended image
            cv::imshow(window_name, blended_image);
        } else if (event == cv::EVENT_RBUTTONDOWN) {
            clicked_points.push_back(Point(x, y, "0"));

            // Perform inference with updated points
            auto result = model->inference(*embedding, drawn_boxes, clicked_points);
            auto daoai_mask_image = result.mask.toImage();

            // Save result to file
            saveResultToFile(result.toJSONString(), *(std::string*)userdata);

            // Convert the mask to OpenCV format
            cv::Mat mask_image(daoai_mask_image.height, daoai_mask_image.width, CV_8UC1, daoai_mask_image.getData());
            mask_image = mask_image.clone();

            // Create a masked image
            cv::Mat masked_image;
            original_image->copyTo(masked_image, mask_image);

            // Blend the original and masked images
            cv::Mat blended_image;
            cv::addWeighted(*original_image, 0.3, masked_image, 0.7, 0, blended_image);

            // Display the blended image
            cv::imshow(window_name, blended_image);
        }
    }

    int main() {
        // Initialize the deep learning environment
        DaoAI::DeepLearning::initialize();

        // Load the image
        std::string image_path = "C:/Users/daoai/test_vision/ins.png"; // Change to your own path
        std::string model_path = "C:/Users/daoai/test_vision/.8/auto_segment.dwm"; // Change to your own path
        cv::Mat image = cv::imread(image_path);
        if (image.empty()) {
            std::cerr << "Error: Could not load the image from " << image_path << std::endl;
            return -1;
        }
        original_image = &image;

        // Load the model and generate embeddings
        try {
            model = new Vision::AutoSegmentation(model_path, DeviceType::GPU);
            Image daoai_image(image_path);
            static auto temp_embedding = model->generateImageEmbeddings(daoai_image);
            embedding = &temp_embedding;
        } catch (const std::exception& e) {
            std::cerr << "Error initializing the model: " << e.what() << std::endl;
            return -1;
        }

        // Create a window to display the image
        cv::namedWindow(window_name, cv::WINDOW_AUTOSIZE);
        cv::imshow(window_name, image);

        // Set the mouse callback function
        cv::setMouseCallback(window_name, onMouse, &image_path);

        // Wait for user interaction
        while (true) {
            int key = cv::waitKey(1);
            if (key == 27) { // Exit on 'Esc' key press
                break;
            } else if (key == 'r' || key == 'R') { // Clear boxes and points on 'r'
                clicked_points.clear();
                drawn_boxes.clear();
                cv::imshow(window_name, *original_image); // Reset to original image
            }
        }

        // Clean up resources
        delete model;
        return 0;
    }


读取文件的变量
----------------

- **image_path**：输入图像的路径（例如 `C:/Users/daoai/test_vision/ins.png`）。
- **model_path**：深度学习模型文件的路径（例如 `C:/Users/daoai/test_vision/.8/auto_segment.dwm`）。

AutoSegmentation 模型
-----------------------

`AutoSegmentation` 模型是 DaoAI 深度学习 SDK 的一部分。该模型旨在根据用户定义的区域（如边界框和点）执行自动图像分割。它可以用于图像分析、物体检测和语义分割等应用。

**主要特点**：

    - **推理**：该模型使用用户提供的边界框和点来生成分割掩码。
    - **嵌入生成**：模型为图像生成嵌入，然后在推理过程中使用这些嵌入。
    - **掩码生成**：模型输出一个分割掩码，识别图像中的兴趣区域。

模型初始化：
------------

- 通过 `.dwm` 文件的路径初始化模型，该文件包含训练好的深度学习模型。可以在 GPU 上运行以加速处理。

加载并使用模型的示例代码：
------------------------------------

.. code-block:: C++

    model = new Vision::AutoSegmentation(model_path, DeviceType::GPU);
    Image daoai_image(image_path);
    static auto temp_embedding = model->generateImageEmbeddings(daoai_image);
    embedding = &temp_embedding;

模型推理：
------------------------------------ 

在与图像交互并选择感兴趣的区域后，进行推理如下：

.. code-block:: C++

    auto result = model->inference(*embedding, drawn_boxes, clicked_points);

这将生成包含分割掩码的结果，可以进行可视化并保存。

保存结果
----------------

分割结果以 JSON 格式保存。该文件包含有关边界框、用户点击的点以及模型生成的掩码的信息。

.. code-block:: C++

    result.toJSONString();

.. code-block:: json

    {
    "Confidence": 0.4031245708465576,
    "Mask": "iVBORw0KGgoAAAANSUhEUgAAB4AAAASwCAAAAAA/WwgqAAAWdElEQVR4Ae3BC1IjVoIEwKr7H/otNP0RoN6wMC......" 
    "ImageHeight": 1200,
    "ImageWidth": 1920
    }

其中 Mask 为 Base64 字符串 代表了分割出的掩膜

故障排除
--------

- **图像加载错误**：如果图像路径不正确或文件丢失，请确保文件存在并且路径正确。
- **模型初始化错误**：如果模型路径不正确或文件损坏，请确保模型正确加载并且文件存在于指定位置。

总结
--------

该程序提供了一个易于使用的界面，供用户进行交互式图像分割，利用 DaoAI 的深度学习模型根据用户输入进行物体分割。通过实时反馈和结果保存，它是进行图像分析和计算机视觉任务的强大工具。
