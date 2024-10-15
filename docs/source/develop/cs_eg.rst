C# Code Examples
======================

This chapter provides detailed examples of C# code included in the DaoAI World SDK.

Including Libraries
-----------------------

In the C# example, we include the following libraries, with ``DaoAI.DeepLearningCLI`` being the library for the DaoAI World SDK.

.. code-block:: C#

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Drawing;
    using System.Threading.Tasks;
    using DaoAI.DeepLearningCLI;


Reading Images
-----------------

The model prediction function in the DaoAI World SDK requires the image to be represented as a one-dimensional array (1D array). Below is the code to read an image from a file:

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
    

Here, we make a deep copy of the image and then use the ``DaoAI.DeepLearningCLI.Image`` function to initialize the image object for later use:

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

Loading Deep Learning Models
-------------------------------

.. code-block:: C#

        String data_path = "..\\..\\..\\..\\Data\\";
        String model_path = data_path + "model.dwm";
        // init model
        DaoAI.DeepLearningCLI.Vision.KeypointDetection model = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);


Note that each detection task has a corresponding object:

.. code-block:: C#

    //Instance Segmentation
    DaoAI.DeepLearningCLI.Vision.InstanceSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.InstanceSegmentation(model_path);

    //Keypoint Detection
    DaoAI.DeepLearningCLI.Vision.KeypointDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.KeypointDetection(model_path);
    
    //Image Classification
    DaoAI.DeepLearningCLI.Vision.Classification model(model_path) = new DaoAI.DeepLearningCLI.Vision.Classification(model_path);
    
    //Object Detection
    DaoAI.DeepLearningCLI.Vision.ObjectDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.ObjectDetection(model_path);
    
    //Unsupervised Defect Segmentation
    DaoAI.DeepLearningCLI.Vision.AnomalyDetection model(model_path) = new DaoAI.DeepLearningCLI.Vision.AnomalyDetection(model_path);
    
    //Supervised Defect Segmentation
    DaoAI.DeepLearningCLI.Vision.SemanticSegmentation model(model_path) = new DaoAI.DeepLearningCLI.Vision.SemanticSegmentation(model_path);
    
    //OCR
    DaoAI.DeepLearningCLI.Vision.OCR model(model_path) = new DaoAI.DeepLearningCLI.Vision.OCR(model_path);

    //Positioning (Only Available in Industrial Version)
    DaoAI.DeepLearningCLI.Vision.Positioning model(model_path) = new DaoAI.DeepLearningCLI.Vision.Positioning(model_path);

    //Presence Checking (Only Available in Industrial Version)
    DaoAI.DeepLearningCLI.Vision.PresenceChecking model(model_path) = new DaoAI.DeepLearningCLI.Vision.PresenceChecking(model_path);


Using Deep Learning Models for Prediction
----------------------------------------------

Here, we define a confidence threshold (CONFIDENT_THRESHOLD) of 0.5 and call the model.inference() function to perform inference with the model, then use the .toJSONString() method to print the result as JSON.

.. code-block:: C#

    Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object> post_params = new Dictionary<DaoAI.DeepLearningCLI.PostProcessType, object>();
    post_params[DaoAI.DeepLearningCLI.PostProcessType.CONFIDENT_THRESHOLD] = 0.5;

    Console.WriteLine(model.inference(img, post_params).toJSONString());

Example of Returned Results
----------------------------------

Below is an example of the results returned by the instance segmentation model.

This result shows the predicted results, including number of detections, bounding box, labels, and masks.

This information can be used for further processing or analysis of the prediction results.

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