DaoAI 非监督缺陷检测SDK
======================================

DaoAI 非监督缺陷检测SDK 提供了一套完整的工具，帮助用户加载预训练模型进行推理，或者通过用户提供的图像数据，自主训练模型并执行缺陷检测。

DaoAI 非监督缺陷检测SDK 支持 **C++**

您也可以查看我们的 GitHub repo，其中包含 C++ 的非监督缺陷检测SDK示例代码。

链接： `DaoAI World SDK Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_

安装和准备工作
----------------

在开始使用 SDK 之前，请确保您已经下载并安装了必要的工具包：

需要下载 `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_，并在 **Unsupervised Defect Segmentation SDK** 目录下找到 **2.22.8.0** 的安装包 ZIP 文件。

功能概览
--------

1. **加载预训练模型并进行推理**（见 main2 函数）
   使用从 DaoAI World 平台下载的预训练模型，SDK 可以执行以下操作：

   - 支持像素级和图像级的推理。
   - 提供推理结果，包括异常评分和注释。

2. **自主训练模型并进行推理**（见 main 函数）
   用户可以通过提供好的和坏的样本数据，让 SDK 自主训练一个用于缺陷检测的图像级或者像素级的模型。

使用说明
--------

以下代码展示了如何加载从 DaoAI World 平台下载的预训练模型，并使用该模型对图像进行推理。代码的各个部分功能如下：


第一部分：加载预训练模型并推理
--------------------------------

以下代码展示了如何加载从 DaoAI World 平台下载的预训练模型，并使用该模型对图像进行推理：

.. code-block:: cpp

   #include <dlsdk/utils.h>
   #include <dlsdk/model.h>
   #include <iostream>
   #include <fstream>

   using namespace DaoAI::DeepLearning;

    int main2()
    {
        try {
            // Initialize Unsupervised library
            initialize();

            // Configurate the model and data path
            std::string root_directory = "C:/Users/daoai/test_vision/";  // Change to your own directory

            // Construct the model on speficied device
            Vision::UnsupervisedDefectSegmentation model(DeviceType::GPU);
            model.addComponentArchive(root_directory + "unsup_img_whole.dwm");
            std::cout << model.getBatchSize() << std::endl;

            // Set batch size
            model.setBatchSize(1);

            std::string img_path = root_directory + "unsup_img_whole (1).png";  // Change to your own directory
            Image img(img_path);

            Vision::UnsupervisedDefectSegmentationResult result = model.inference(img);

            // Print the result
            std::cout << "Anomaly score: " << result.confidence << std::endl;
            std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n\n";

            // Save the result to a file
            std::string file_path = root_directory + "output.json";
            std::ofstream output_file(file_path);
            if (output_file.is_open()) {
                output_file << result.toAnnotationJSONString();
                output_file.close();
                std::cout << "JSON result saved to: " << file_path << std::endl;
            } else {
                std::cerr << "Failed to open the file: " << file_path << std::endl;
            }

            return 0;
        }
        catch (const std::exception& e) {
            std::cout << "Caught an exception: " << e.what() << std::endl;
            return -1;
        }
    }

代码说明
^^^^^^^^^

1. **初始化 Unsupervised 库**

   .. code-block:: cpp

      initialize();

   **功能**：初始化 Unsupervised 库，为后续模型加载和推理操作做好准备。

2. **配置模型路径和数据路径**

   .. code-block:: cpp

      std::string root_directory = "C:/Users/daoai/test_vision/";

   **功能**：设置模型文件和图像文件的根目录路径，用户需要根据实际情况更改。

3. **加载预训练模型**

   .. code-block:: cpp

      Vision::UnsupervisedDefectSegmentation model(DeviceType::GPU);
      model.addComponentArchive(root_directory + "unsup_img_whole.dwm");

   **功能**：加载预训练的模型组件，支持 GPU 设备以提升推理效率。

4. **设置批量大小**

   .. code-block:: cpp

      model.setBatchSize(1);

   **功能**：设置模型推理的批量大小，这里设置为 1。

5. **加载输入图像**

   .. code-block:: cpp

      std::string img_path = root_directory + "unsup_img_whole (1).png";
      Image img(img_path);

   **功能**：加载需要进行推理的图像文件。

6. **进行推理并输出结果**

   .. code-block:: cpp

      Vision::UnsupervisedDefectSegmentationResult result = model.inference(img);
      std::cout << "Anomaly score: " << result.confidence << std::endl;
      std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n\n";

   **功能**：对图像进行推理，输出异常分数（Anomaly score）和 JSON 格式的推理结果。

7. **保存推理结果**

   .. code-block:: cpp

      std::ofstream output_file(file_path);
      if (output_file.is_open()) {
          output_file << result.toAnnotationJSONString();
          output_file.close();
          std::cout << "JSON result saved to: " << file_path << std::endl;
      } else {
          std::cerr << "Failed to open the file: " << file_path << std::endl;
      }

   **功能**：将推理结果保存为 JSON 文件，方便后续分析。

8. **异常处理**

   .. code-block:: cpp

      catch (const std::exception& e) {
          std::cout << "Caught an exception: " << e.what() << std::endl;
          return -1;
      }

   **功能**：捕获可能出现的异常并打印错误信息。


第二部分：自主训练模型并推理
--------------------------

以下代码展示了如何使用用户提供的样本数据，自主训练模型并进行推理：

第二部分使用了opencv库来绘制掩膜，请确保安装了opencv库。

.. code-block:: cpp

   #include <dlsdk/utils.h>
   #include <dlsdk/model.h>
   #include <iostream>
   #include <fstream>
   #include <opencv2/opencv.hpp>

   using namespace DaoAI::DeepLearning;

    int main()
    {
        try {
            // Initialize Unsupervised library
            initialize();

            // Configurate the model and data path
            std::string root_directory = "C:/Users/daoai/test_vision/";  // Change to your own directory
            std::string data_path = "C:/Users/daoai/test_vision/ano/";  // Change to your own data directory

            // Load images
            std::vector<Image> good_images;
            for (auto& file : std::filesystem::directory_iterator(data_path + "good"))
            {
                if (file.path().extension() == ".png")
                {
                    good_images.push_back(Image(file.path().string()));
                }
            }

            std::vector<Image> bad_images;
            std::vector<Image> masks;
            for (auto& file : std::filesystem::directory_iterator(data_path + "bad"))
            {
                if (file.path().extension() == ".png")
                {
                    Image image(file.path().string());
                    bad_images.push_back(image);

                    // Create a binary mask
                    cv::Mat maskMat = cv::Mat::zeros(image.height, image.width, CV_8UC1);
                    int centerX = image.width / 2;
                    int centerY = image.height / 2;
                    int radius = static_cast<int>(image.width * 0.25);
                    cv::circle(maskMat, cv::Point(centerX, centerY), radius, cv::Scalar(255), -1);

                    Image mask(maskMat.rows, maskMat.cols, DaoAI::Unsupervised::Image::Type::GRAYSCALE, maskMat.data);
                    masks.push_back(mask.clone());
                }
            }

            // Construct the model
            Vision::UnsupervisedDefectSegmentation model(DeviceType::GPU);
            model.setDetectionLevel(DetectionLevel::PIXEL);

            ComponentMemory component;
            try
            {
                component = model.createComponentMemory("screw", good_images, bad_images, masks, true);
                component.save(data_path + "component_1.pth");
                model.setBatchSize(1);
            }
            catch (std::exception& e)
            {
                std::cout << e.what() << "\n";
            }

            Vision::UnsupervisedDefectSegmentationResult result = model.inference(bad_images[0]);

            std::cout << "Anomaly score: " << result.confidence << std::endl;
            std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n";
            return 0;
        }
        catch (const std::exception& e) {
            std::cout << "Caught an exception: " << e.what() << std::endl;
            return -1;
        }
    }


代码功能：通过用户提供的样本数据训练模型，并使用训练后的模型进行推理。

代码说明
^^^^^^^^^

1. **初始化 Unsupervised 库**

   .. code-block:: cpp

      initialize();

   **功能**：与前面相同，用于初始化 Unsupervised 库。

2. **设置模型和数据路径**

   .. code-block:: cpp

      std::string root_directory = "C:/Users/daoai/test_vision/";
      std::string data_path = "C:/Users/daoai/test_vision/ano/";

   **功能**：设置存储模型文件和样本数据的路径。

3. **加载“好”样本图像**

   .. code-block:: cpp

      for (auto& file : std::filesystem::directory_iterator(data_path + "good")) {
          if (file.path().extension() == ".png") {
              good_images.push_back(Image(file.path().string()));
          }
      }

   **功能**：加载“好”样本图像到内存中，用于模型训练。

4. **加载“坏”样本图像和掩码**

   .. code-block:: cpp

      for (auto& file : std::filesystem::directory_iterator(data_path + "bad")) {
          if (file.path().extension() == ".png") {
              Image image(file.path().string());
              bad_images.push_back(image);

              // 创建二值掩码
              cv::Mat maskMat = cv::Mat::zeros(image.height, image.width, CV_8UC1);
              int centerX = image.width / 2;
              int centerY = image.height / 2;
              int radius = static_cast<int>(image.width * 0.25);
              cv::circle(maskMat, cv::Point(centerX, centerY), radius, cv::Scalar(255), -1);

              Image mask(maskMat.rows, maskMat.cols, DaoAI::Unsupervised::Image::Type::GRAYSCALE, maskMat.data);
              masks.push_back(mask.clone());
          }
      }

   **功能**：加载“坏”样本图像，同时生成相应的二值掩码，用于训练过程。

5. **构建模型并训练**

   .. code-block:: cpp

      ComponentMemory component = model.createComponentMemory("screw", good_images, bad_images, masks, true);
      component.save(data_path + "component_1.pth");

   **功能**：使用提供的样本数据训练模型，并保存训练后的模型组件。

    结尾的 true 参数，意味着在训练结束后将模型加载至内存使用，默认为false, 则不会加载进内存，如果设为了false,则需要调用 addComponentMemory(file_path)  来加载训练好的模型，再进行推理。

6. **推理并输出结果**

   .. code-block:: cpp

      Vision::UnsupervisedDefectSegmentationResult result = model.inference(bad_images[0]);
      std::cout << "Anomaly score: " << result.confidence << std::endl;
      std::cout << "JSON result: " << result.toAnnotationJSONString() << "\n";

   **功能**：使用训练后的模型对测试图像进行推理，输出异常分数和 JSON 格式的结果。

7. **异常处理**

   .. code-block:: cpp

      catch (const std::exception& e) {
          std::cout << "Caught an exception: " << e.what() << std::endl;
          return -1;
      }

   **功能**：捕获可能出现的异常并打印错误信息。

总结
----

通过 DaoAI 非监督缺陷检测SDK，用户可以轻松地加载预训练模型执行高效推理，或利用自身数据进行自主训练以适应特定需求。
