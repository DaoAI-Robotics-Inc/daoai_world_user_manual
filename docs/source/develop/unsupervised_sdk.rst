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

    int main()
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

代码请参考 github 仓库 `非监督像素级示例代码  <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo/blob/2.24.8.0/cpp_demos/DW_SDK_Cpp_Example/DW%20UnsupervisedDefectSegmentation/main.cpp>`_

代码功能：通过用户提供的样本数据训练模型，并使用训练后的模型进行推理。

使用方法
^^^^^^^^^^^

1. **启动应用程序**  或在 visual studio 中开始调试

2. **提供图像文件夹路径**  
   输入包含图像的文件夹路径（支持 `.png`、`.jpg`、`.jpeg` 格式）。

3. **交互式标注界面**  
   使用 GUI 进行图像标注：

- **按键操作**：

  - `n`: 切换到下一张图像
  - `p`: 返回到上一张图像
  - `g`: 将图像标记为 GOOD（良品）
  - `b`: 将图像标记为 BAD（不良品，允许多边形标注）
  - `r`: 重置多边形标注
  - `f`: 完成多边形（将最后一个点与第一个点连接）
  - `q`: 退出标注工具

- **鼠标操作**：

  - **左键点击**: 在图像上添加多边形标注点（针对不良品图像）
  - **鼠标滚轮**: 放大/缩小图像

4. **保存标注结果**  
   退出标注界面后，工具将自动执行以下操作：

- 将 GOOD（良品）图像复制到 `out/good` 目录。
- 将 BAD（不良品）图像复制到 `out/bad` 目录。
- 生成 BAD 图像的二进制掩码，并保存在 `out/masks` 目录中。

5. **加载标注结果进行训练**  
   工具将从 `out` 目录中重新加载已标注的图像和掩码，准备训练数据。

6. **构建训练组件**  
   工具利用 DaoAI 的 **UnsupervisedDefectSegmentation** （无监督缺陷分割）模型，根据标注数据创建训练组件。

7. **模型推理**  
   使用训练组件对新的图像数据进行推理，并输出缺陷检测结果。


总结
----

通过 DaoAI 非监督缺陷检测SDK，用户可以轻松地加载预训练模型执行高效推理，或利用自身数据进行自主训练以适应特定需求。
