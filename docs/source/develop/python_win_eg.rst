Python Windows Code Example
-----------------------------------

You can use the provided `Python示例代码 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/Elcb0srODHNGpDZYQu58mZsBeoD1173XVKj0YIvUalUGPA?e=OoBkUN>`_ which includes image reading, model loading, prediction, and output.

To run the script, you'll need a valid DaoAI license. If you don't have one, please refer to :ref:`DW SDK License` 

To execute the Python script, use the following command:

.. code-block:: python

    python example.py

You can adjust the file paths in ``example.py`` to use different images and deep learning models.

.. code-block:: python

    model_path = "./model.dwm"
    image_path = "./image.png"

Start by importing the DaoAI World Python Windows SDK 

.. code-block:: python

    import os
    import sys
    import dlsdk.dlsdk as dlsdk

and other useful libraries:

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


Initialize and Load the Model

.. code-block:: python

    # Initialize and Load the Model
    dlsdk.initialize()
    model_path = "./model.dwm"
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)


Model Objects for Different Tasks

.. code-block:: python

    #instance segmentation
    model = dlsdk.InstanceSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    #keypoint detection
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #image classification
    model = dlsdk.Classification(model_path, device=dlsdk.DeviceType.GPU)
    
    #object detection
    model = dlsdk.ObjectDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #anomaly detection
    model = dlsdk.AnomalyDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #semantic segmentation
    model = dlsdk.SemanticSegmentation(model_path, device=dlsdk.DeviceType.GPU)
    
    #OCR
    model = dlsdk.OCR(model_path, device=dlsdk.DeviceType.GPU)

    #Positioning (Only Available in Industrial Version)
    model = dlsdk.Positioning(model_path, device=dlsdk.DeviceType.GPU)

    #Presence Checking (Only Available in Industrial Version)
    model = dlsdk.PresenceChecking(model_path, device=dlsdk.DeviceType.GPU)

You can use OpenCV to read the image. If OpenCV is not installed, you can install it using ``pip install python-opencv``

.. code-block:: python

    image_path = "./kp1.png" # Path to your image file
    img = cv2.imread(image_path)

    daoai_image = dlsdk.Image.from_numpy(img, dlsdk.Image.Type.BGR) # Create DaoAI Image

Making Predictions and Outputting Results

.. code-block:: python

    assert isinstance(daoai_image, dlsdk.Image)
    prediction = model.inference(daoai_image,{dlsdk.PostProcessType.CONFIDENCE_THRESHOLD: 0.95})

    with open("output.json", "w") as f:
        f.write(prediction.toJSONString()) # output regular results in json format

    with open("outputAnnotation.json", "w") as f:
        f.write(prediction.toAnnotationJSONString()) # output annotation formatted results to JSON string.

Accessing Prediction Results

.. code-block:: python

    print(prediction.boxes)
    print(prediction.class_ids) 
    print(prediction.class_labels)