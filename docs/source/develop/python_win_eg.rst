Python Windows Code Example
----------------------------

You can use the provided `Python example code <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/Elcb0srODHNGpDZYQu58mZsBeoD1173XVKj0YIvUalUGPA?e=OoBkUN>`_ which includes image reading, model loading, model prediction, and output.

You can also explore our GitHub repository, which includes example projects in C++, C#, and Python to help users get started quickly and easily.

Link: DaoAI World SDK Desktop Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>_

.. contents::
    :local:

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

    #Rotated Object Detection
    model = dwsdk.RotatedObjectDetection(model_path, device=dwsdk.DeviceType.GPU)

    #Mixed Model
    model = dwsdk.MultilabelDetection(model_path, device=dwsdk.DeviceType.GPU)

    # Unsupervised Defect Segmentation, only available in DaoAI Unsupervised SDK
    
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

The model's predictions can accept post-processing parameters:

    - model.setConfidenceThreshold()

        Confidence threshold, which filters out results with confidence lower than the specified value.

    - model.setIOUThreshold()

        IOU (Intersection over Union) threshold, which filters out results with IOU lower than the specified value.

.. code-block:: python

    model.setConfidenceThreshold(0.5)
    prediction = model.inference(daoai_image)

You can also retrieve result information using other methods:

.. code-block:: python

    print(prediction.boxes)
    print(prediction.class_ids)
    print(prediction.class_labels)


Retrieving Prediction Results
******************************

Box (Bounding Box)
^^^^^^^^^^^^^^^^^^^^

The `Box` represents the bounding box of the predicted results from the model. You can retrieve it as follows:

.. code-block:: python

    prediction = model.inference(daoai_image)

    prediction.boxes[0].x1()  # Top-left corner X-coordinate
    prediction.boxes[0].y1()  # Top-left corner Y-coordinate
    prediction.boxes[0].x2()  # Bottom-right corner X-coordinate
    prediction.boxes[0].y2()  # Bottom-right corner Y-coordinate

Mask (Contour)
^^^^^^^^^^^^^^^^^^^^

`Mask` provides the contour information of the target object in the prediction. You can extract the vertices of the polygonal region as follows:

.. code-block:: python

    prediction = model.inference(daoai_image)

    prediction.masks[0].toPolygons()[0].points[0].x  # X-coordinate of the vertex
    prediction.masks[0].toPolygons()[0].points[0].y  # Y-coordinate of the vertex

- **`masks[0]`**: Extracts the mask of the first target object.
- **`toPolygons()`**: Converts the mask into polygonal contours.
- **`points[0]`**: Retrieves the first vertex of the polygon.

By connecting all the vertices sequentially, you can form the complete contour of the target object. This is useful for applications requiring detailed boundary information, such as region analysis or fine-grained annotations.

Visualization Output
^^^^^^^^^^^^^^^^^^^^^

Generate and save a visualization image that overlays the prediction results on the original image:

.. code-block:: python

    result = dlsdk.visualize(daoai_image, prediction)

    result.save("output.png")

