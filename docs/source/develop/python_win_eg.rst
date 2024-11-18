Python Windows Code Example
----------------------------

You can use the provided `Python example code <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/Elcb0srODHNGpDZYQu58mZsBeoD1173XVKj0YIvUalUGPA?e=OoBkUN>`_ which includes image reading, model loading, model prediction, and output.

You will need a valid DaoAI license to run the code. If you do not have a license, please refer to :ref:`software license`.

Then, run the following command to execute the Python script:

.. code-block:: python

    python example.py

You can change the file paths in `example.py` to use different images and deep learning models.

.. code-block:: python

    model_path = "./model.dwm"
    image_path = "./image.png"


Import Libraries
~~~~~~~~~~~~~~~~

You can also start with a new Python file. First, import the `dlsdk` library, which is our DaoAI World Python Windows SDK.

.. code-block:: python

    import os
    import sys
    import dlsdk.dlsdk as dlsdk

The following libraries may also be helpful:

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


Loading a Deep Learning Model
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    # Initialize the model
    dlsdk.initialize()
    model_path = "./model.dwm"
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)

Note that each detection task corresponds to a specific object:

.. code-block:: python

    # Instance Segmentation
    model = dlsdk.InstanceSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    # Keypoint Detection
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    # Image Classification
    model = dlsdk.Classification(model_path, device=dlsdk.DeviceType.GPU)
    
    # Object Detection
    model = dlsdk.ObjectDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    # Anomaly Detection (for versions before .6, renamed to Unsupervised Defect Detection in .7)
    model = dlsdk.AnomalyDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    # Semantic Segmentation (for versions before .6, renamed to Supervised Defect Detection in .7)
    model = dlsdk.SemanticSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    # Unsupervised Defect Segmentation
    model = dlsdk.UnsupervisedDefectSegmentation(model_path, device=dlsdk.DeviceType.GPU)
    
    # Supervised Defect Segmentation
    model = dlsdk.SupervisedDefectSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    # OCR
    model = dlsdk.OCR(model_path, device=dlsdk.DeviceType.GPU)

    # Positioning Model (only supported in the industrial version)
    model = dlsdk.Positioning(model_path, device=dlsdk.DeviceType.GPU)

    # Presence Checking (only supported in the industrial version)
    model = dlsdk.PresenceChecking(model_path, device=dlsdk.DeviceType.GPU)


Reading Images
~~~~~~~~~~~~~~~~

To read images, you can use OpenCV. If you do not have it installed, you can run ``pip install python-opencv`` to install it.

.. code-block:: python

    image_path = "./kp1.png"  # Path to the image being read
    img = cv2.imread(image_path)

    daoai_image = dlsdk.Image.from_numpy(img, dlsdk.Image.Type.BGR)  # Create DaoAI Image 


Running Deep Learning Model Predictions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Make model predictions and output results as a JSON file.

.. code-block:: python

    assert isinstance(daoai_image, dlsdk.Image)
    prediction = model.inference(daoai_image, {dlsdk.PostProcessType.CONFIDENCE_THRESHOLD: 0.95})

    with open("output.json", "w") as f:
        f.write(prediction.toJSONString())  # Output standard JSON result

    with open("outputAnnotation.json", "w") as f:
        f.write(prediction.toAnnotationJSONString())  # Output annotated JSON result

Post-Processing
***************

Model predictions can accept post-processing parameters:

    - dlsdk.PostProcessType.CONFIDENCE_THRESHOLD 
      
      Confidence threshold, which will filter out results with a confidence below the set value.
      
    - dlsdk.PostProcessType.IOU_THRESHOLD 
      
      IOU threshold, which will filter out results with IOU below the set value.

    - dlsdk.PostProcessType.SENSITIVITY_THRESHOLD 
      
      Used in unsupervised defect segmentation (anomaly detection), it controls the model's sensitivity to defects. The higher the value, the more defects the model will detect, but it may also increase false positives.

.. code-block:: python

    prediction = model.inference(daoai_image, {dlsdk.PostProcessType.CONFIDENCE_THRESHOLD: 0.95, dlsdk.PostProcessType.IOU_THRESHOLD: 0.5})


You can also retrieve result information using other methods:

.. code-block:: python

    print(prediction.boxes)
    print(prediction.class_ids)
    print(prediction.class_labels)
