C# Code Example
==================

This chapter provides a detailed explanation of the C# code examples included in the DaoAI World SDK.

.. contents::
    :local:

Importing Libraries
------------------------

In the C# example, the following libraries are imported, with ``DaoAI.DeepLearningCLI`` being the library used to import the DaoAI World SDK.

.. code-block:: C#

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Drawing;
    using System.Threading.Tasks;
    using DaoAI.DeepLearningCLI;

Setting Environment Variables
-----------------------------------

In most cases, the system environment variables required for DW SDK are pre-configured during install. However, in rare cases, you might encounter other paths containing conflicting dynamic link libraries.

You can use DW SDK without modifying the system environment variables. Simply set the environment variables at the beginning of your code:

.. code-block:: csharp

    Environment.SetEnvironmentVariable(
        "PATH",
        Environment.GetEnvironmentVariable("DWSDK_PATH") + @"\bin;" +
        Environment.GetEnvironmentVariable("DWSDK_PATH") + @"\3rd_party;" +
        Environment.GetEnvironmentVariable("PATH"),
        EnvironmentVariableTarget.Process
    );

Reading an Image
----------------

The model prediction function in the DaoAI World SDK requires the image to be represented as a one-dimensional array (1D array). Here is the code for reading an image from a file:

.. code-block:: C#

    // Test image
    String root_directory = System.IO.Directory.GetCurrentDirectory();

    System.Drawing.Bitmap image = new
        System.Drawing.Bitmap("C:\\Users\\daoai\\Downloads\\DW_SDK\\DW_SDK Example\\Data\\maskrcnn_data\\daoai_1.png"); //Image file path
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
    

Here, a deep copy of the image is made, and the image object is initialized using the `DaoAI.DeepLearningCLI.Image` function for further use:

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

Loading a Deep Learning Model
-----------------------------

.. code-block:: C#

        String data_path = "..\\..\\..\\..\\Data\\"; 
        String model_path = data_path + "model.dwm"; 
        // Initialize model
        DaoAI.DeepLearningCLI.Vision.KeypointDetection model = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);

Note that each detection task corresponds to a specific object:

.. code-block:: C#

    // Instance segmentation
    DaoAI.DeepLearningCLI.Vision.InstanceSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.InstanceSegmentation(model_path);

    // Keypoint detection
    DaoAI.DeepLearningCLI.Vision.KeypointDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);
    
    // Image classification
    DaoAI.DeepLearningCLI.Vision.Classification model(model_path) = new DaoAI.DeepLearningCLI.Vision.Classification(model_path);
    
    // Object detection
    DaoAI.DeepLearningCLI.Vision.ObjectDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.ObjectDetection(model_path);

    // Unsupervised defect detection
    DaoAI.DeepLearningCLI.Vision.UnsupervisedDefectSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.AnomalyDetection(model_path);
    
    // Supervised defect detection
    DaoAI.DeepLearningCLI.Vision.SupervisedDefectSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.SemanticSegmentation(model_path);

    // OCR
    DaoAI.DeepLearningCLI.Vision.OCR model(model_path) = new DaoAI.DeepLearningCLI.Vision.OCR(model_path);

    // Positioning model (available only in the industrial version)
    DaoAI.DeepLearningCLI.Vision.Positioning model(model_path) = new DaoAI.DeepLearningCLI.Vision.Positioning(model_path);

    // Error detection (available only in the industrial version)
    DaoAI.DeepLearningCLI.Vision.PresenceChecking model(model_path) = new DaoAI.DeepLearningCLI.Vision.PresenceChecking(model_path);


Using a Deep Learning Model for Prediction
------------------------------------------

Here, the confidence threshold (CONFIDENT_THRESHOLD) is set to 0.5, and the `model.inference()` function is called to perform inference with the model. The results are then printed as a JSON using the `.toJSONString()` method:

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENT_THRESHOLD] = 0.5;

    Console.WriteLine(model.inference(img, post_params).toJSONString());

Post-processing Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~

The model's predictions can accept post-processing parameters:

    - DaoAI.DeepLearningCLI.PostProcessType.CONFIDENCE_THRESHOLD:

        The confidence threshold filters out results with a confidence lower than the specified value.
    
    - post_params[DaoAI.DeepLearningCLI.PostProcessType.IOU_THRESHOLD]:

        The IOU threshold filters out results with an IOU lower than the specified value.
   
    - post_params[DaoAI.DeepLearningCLI.PostProcessType.SENSITIVITY_THRESHOLD]:

        Used in unsupervised defect segmentation (anomaly detection) models to control the sensitivity to defects. The higher the sensitivity, the more defects the model will detect, but it may result in more false positives.

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENCE_THRESHOLD] = 0.5; // Filters out results with confidence lower than the set value
    post_params[DaoAI.DeepLearningCLI.PostProcessType.IOU_THRESHOLD] = 0.5; // Filters out results with IOU lower than the set value

    Console.WriteLine(model.inference(img, post_params));

Retrieving Prediction Results
-----------------------------

Box (Bounding Box)
~~~~~~~~~~~~~~~~~~

The `Box` represents the bounding box of the predicted results from the model. You can retrieve it as follows:

.. code-block:: csharp

    // Retrieve prediction results
    DaoAI.DeepLearningCLI.Vision.KeypointDetectionResult prediction = model.inference(daoaiImage);

    // Access bounding box coordinates
    double x1 = prediction.boxes[0].x1();  // Top-left corner X-coordinate
    double y1 = prediction.boxes[0].y1();  // Top-left corner Y-coordinate
    double x2 = prediction.boxes[0].x2();  // Bottom-right corner X-coordinate
    double y2 = prediction.boxes[0].y2();  // Bottom-right corner Y-coordinate

Mask (Contour)
~~~~~~~~~~~~~~

`Mask` provides the contour information of the target object in the prediction. You can extract the vertices of the polygonal region as follows:

.. code-block:: csharp

    // Retrieve prediction results
    DaoAI.DeepLearningCLI.Vision.KeypointDetectionResult prediction = model.inference(daoaiImage);

    // Extract the first vertex of the polygonal region
    double x = prediction.masks[0].toPolygons()[0].points[0].X; // X-coordinate of the vertex
    double y = prediction.masks[0].toPolygons()[0].points[0].Y; // Y-coordinate of the vertex

- **`masks[0]`**: Extracts the mask of the first target object.  
- **`toPolygons()`**: Converts the mask into polygonal contours.  
- **`points[0]`**: Retrieves the first vertex of the polygon.  

By connecting all the vertices sequentially, you can form the complete contour of the target object. This method is suitable for scenarios that require detailed boundary information, such as region analysis or fine-grained annotations.

Visualization Output
~~~~~~~~~~~~~~~~~~~~

Generate and save a visualization image that overlays the prediction results on the original image:

.. code-block:: csharp

    DaoAI.DeepLearningCLI.Image result = DaoAI.DeepLearningCLI.Utils.Visualize(img, prediction);
    result.save(rootDirectory + "daoai_output.png");


Example of Returned Results
----------------------------

Below is an example of the results returned after running an instance segmentation model prediction.

This result shows the predicted quantity, label names, confidence, bounding boxes, and polygon masks.

.. code-block:: json

    {
        "Number of detections": 1,
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
