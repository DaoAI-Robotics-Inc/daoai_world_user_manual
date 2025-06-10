C++ Auto Segmentation Example Project
===============================================

Overview
--------

This software provides an interactive image viewer that allows users to draw bounding boxes and click points on the image to perform automatic segmentation based on deep learning models. The segmentation results are saved as JSON files and displayed in the window with visual masks overlaid on the original image.

The program uses the `DaoAI AutoSegmentation` model, which performs inference based on the user-drawn bounding boxes and clicked points. The segmentation results are blended with the original image to provide a visual representation of the mask.

The smart segmentation feature is part of the standard local SDK and supports both **C#** and **Python**.

You can also check our GitHub repo, which includes example projects in C++, C#, and Python for easy reference and quick start.

Link: `DaoAI World SDK Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_

Prerequisites
-------------

- **OpenCV v15**: For displaying and interacting with images.
- **DaoAI DWSDK**: For using the `DaoAI AutoSegmentation` model.
- **Deep learning model file** ( `auto_segment.dwm <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EcAQRAxfS6tGqmz21K3SJGIBD27g-aaf5RWvF7Z1MO6v8w?e=R6TGJq>`_ )
- **Image file** to be segmented
- **Download the example project** `Auto Segment C++ Example Project <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/Ed_wyHsxcu9Li_PoXNlsFLsBMFMdycuu4QuE_bIghj-Lrw?e=VXllfb>`_

Installation
------------

1. Ensure the necessary dependencies are installed:
   - OpenCV v15
   - DaoAI Deep Learning SDK

2. Download the deep learning model:
   - The model file should be `auto_segment.dwm <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EcAQRAxfS6tGqmz21K3SJGIBD27g-aaf5RWvF7Z1MO6v8w?e=R6TGJq>`_

3. Prepare the image for segmentation:
   - The image file should be in a standard format supported by OpenCV, such as `.png`, `.jpg`, or `.bmp`.

Usage
-----

1. **Run the Program**:

   - After downloading the example project, unzip it, open `DLSDK Example.sln`, right-click on the project -> Properties, and configure the OpenCV v15 library path in the C++ and Linker tabs.

.. note::

    - On first run, model loading and data initialization may take longer. Subsequent runs (after the first image load) will take about 0.2 seconds for loading.
    - Memory usage is approximately 1GB.

2. **Interacting with the Image**:
   
   - **Left-Click Drag**: Click and drag the mouse to draw a bounding box to segment the object inside the box.
   - **Left-Click**: Click on the image to choose a point for inclusion in the segmentation mask.
   - **Right-Click**: Click on the image to choose a point for exclusion from the segmentation mask.

3. **Execute Segmentation**:
   - When the mouse button is released, the program triggers inference based on the bounding boxes and selected points.
   - The result is processed by the `AutoSegmentation` model, and a segmentation mask is returned.

4. **Save Results**:
   - The segmentation result and inference details are saved in a `result.json` file located in the same directory as the input image.
   - The JSON file contains the segmentation mask and other relevant information.

5. **Reset the Image**:
   - Press the **'r'** key to clear the bounding boxes and selected points, resetting to the original image.

6. **Exit the Program**:
   - Press the **'Esc'** key to exit the program.

Example Code
-------------

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


Reading Variables
-----------------

- **image_path**: Path to the input image (e.g., `C:/Users/daoai/test_vision/ins.png`).
- **model_path**: Path to the deep learning model file (e.g., `C:/Users/daoai/test_vision/.8/auto_segment.dwm`).

AutoSegmentation Model
-----------------------

The `AutoSegmentation` model is part of the DaoAI deep learning SDK. It performs automatic image segmentation based on user-defined regions (e.g., bounding boxes and points).

**Main Features**:

    - **Inference**: Uses bounding boxes and points provided by the user to generate segmentation masks.
    - **Embedding Generation**: The model generates embeddings for images, which are used during inference.
    - **Mask Generation**: The model outputs a segmentation mask identifying regions of interest in the image.

Model Initialization:
---------------------

- The model is initialized via the `.dwm` file, which contains a trained deep learning model. It can be run on the GPU to accelerate processing.

Example Code to Load and Use the Model:
---------------------------------------

.. code-block:: C++

    model = new Vision::AutoSegmentation(model_path, DeviceType::GPU);
    Image daoai_image(image_path);
    static auto temp_embedding = model->generateImageEmbeddings(daoai_image);
    embedding = &temp_embedding;

Model Inference:
-----------------

After interacting with the image and selecting the regions of interest, inference is performed as follows:

.. code-block:: C++

    auto result = model->inference(*embedding, drawn_boxes, clicked_points);

This generates a result that includes the segmentation mask, which can be visualized and saved.

Saving Results
--------------

The segmentation result is saved in JSON format. The file contains information about the bounding boxes, points clicked by the user, and the mask generated by the model.

.. code-block:: C++

    result.toJSONString();

.. code-block:: json

    {
    "Confidence": 0.4031245708465576,
    "Mask": "iVBORw0KGgoAAAANSUhEUgAAB4AAAASwCAAAAAA/WwgqAAAWdElEQVR4Ae3BC1IjVoIEwKr7H/otNP0RoN6wMC......"
    "ImageHeight": 1200,
    "ImageWidth": 1920
    }

Where "Mask" is a Base64 string representing the segmented mask.

Troubleshooting
---------------

- **Image Load Error**: If the image path is incorrect or the file is missing, ensure the file exists and the path is correct.
- **Model Initialization Error**: If the model path is incorrect or the file is corrupted, ensure the model is loaded properly and the file exists at the specified location.

Summary
-------

This program provides an easy-to-use interface for interactive image segmentation, leveraging DaoAI's deep learning model for automatic object segmentation based on user input. With real-time feedback and result saving, it is a powerful tool for image analysis and computer vision tasks.
