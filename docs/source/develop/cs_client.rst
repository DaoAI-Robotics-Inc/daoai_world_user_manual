C# Inference Client Example Project
====================================

.. contents::
    :local:

This chapter provides a detailed explanation of the C# Inference Client code sample included in the DaoAI World SDK.

Import Libraries
-------------------

In the C# example, we use the following libraries:

.. code-block:: C#

    using System;
    using System.IO;
    using DaoAI.InferenceClient;

Read Images
--------------

The model inference function of the C# Inference Client requires the image to be represented as a base64-encoded string. You can use the `Convert.ToBase64String()` method for conversion.

First, define the file path. The format should be a `.txt` file containing the base64-encoded image, and then read it.

.. code-block:: C#

    string fileName = "C:/Users/daoai/Documents/DWSDK_Demo/data/image.bmp";

    string base64Image = Convert.ToBase64String(File.ReadAllBytes(fileName));

Load the Deep Learning Model
------------------------------

The deep learning models exported by DaoAI World are usually in `.dwm` format. You need to create a `DaoAI.InferenceClient.KeypointDetection` object and use the constructor to load the model.

.. code-block:: C#

    // model path in server file system
    string filemodel = "../../../../../../data/KeypointDetection.dwm";

    DaoAI.InferenceClient.KeypointDetection model = new DaoAI.InferenceClient.KeypointDetection(filePath_model, DaoAI.InferenceClient.DeviceType.GPU);

Note that each detection task has a corresponding object:

.. code-block:: C#

    // Instance Segmentation
    DaoAI.InferenceClient.InstanceSegmentation model(model_path);

    // Keypoint Detection
    DaoAI.InferenceClient.KeypointDetection model(model_path);

    // Classification
    DaoAI.InferenceClient.Classification model(model_path);

    // Object Detection
    DaoAI.InferenceClient.ObjectDetection model(model_path);

    // Unsupervised Defect Detection
    DaoAI.InferenceClient.UnsupervisedDefectSegmentation model(model_path);

    // Supervised Defect Detection
    DaoAI.InferenceClient.SupervisedDefectSegmentation model(model_path);

    // OCR
    DaoAI.InferenceClient.OCR model(model_path);

    // Positioning (Only supported in industrial version)
    DaoAI.InferenceClient.Positioning model(model_path);

    // Presence Checking (Only supported in industrial version)
    DaoAI.InferenceClient.PresenceChecking model(model_path);

If you attempt to load the wrong model type, an error will be thrown, indicating which model type should be used.

Run Inference Using the Model
------------------------------

.. code-block:: C#

    // get inference
    DaoAI.InferenceClient.KeypointDetection prediction = model.inference(base64Image);

Note that each detection task returns a corresponding result object:

.. code-block:: C#

    // Instance Segmentation
    DaoAI.InferenceClient.InstanceSegmentationResult prediction = model.inference(base64Image);

    // Keypoint Detection
    DaoAI.InferenceClient.KeypointDetectionResult prediction = model.inference(base64Image);

    // Classification
    DaoAI.InferenceClient.ClassificationResult prediction = model.inference(base64Image);

    // Object Detection
    DaoAI.InferenceClient.ObjectDetectionResult prediction = model.inference(base64Image);

    // Anomaly Detection
    DaoAI.InferenceClient.AnomalyDetectionResult prediction = model.inference(base64Image);

    // Semantic Segmentation
    DaoAI.InferenceClient.SemanticSegmentationResult prediction = model.inference(base64Image);

    // OCR
    DaoAI.InferenceClient.OCRResult prediction = model.inference(base64Image);

    // Positioning (Only supported in industrial version)
    DaoAI.InferenceClient.PositioningResult prediction = model.inference(base64Image);

    // Presence Checking (Only supported in industrial version)
    DaoAI.InferenceClient.PresenceCheckingResult prediction = model.inference(base64Image);

Sample Output
------------------

Below is a sample output from the keypoint detection model inference.

This output shows the number of detections, class labels, confidence scores, bounding boxes, keypoints, and polygon masks.

.. code-block:: C#

    for (int i = 0; i < result.NumDetections; i++)
    {
        Console.WriteLine($"Object {i + 1}");
        Console.WriteLine($"Class: {result.ClassLabels[i]}");
        Console.WriteLine($"Bounding box: {result.Boxes[i].X1} {result.Boxes[i].Y1} {result.Boxes[i].X2} {result.Boxes[i].Y2}");
        Console.WriteLine($"Confidence: {result.Confidences[i]}");
        Console.WriteLine("Keypoints:");

        foreach (var keypoint in result.Keypoints[i])
        {
            Console.WriteLine($"{keypoint.X} {keypoint.Y} {keypoint.Confidence}");
        }
        Console.WriteLine();
    }
