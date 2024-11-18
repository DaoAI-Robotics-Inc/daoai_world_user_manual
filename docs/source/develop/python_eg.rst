Python Linux/Jetson Code Example
-----------------------------------

You can use the provided `Python example code <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/Ed6ajuVWvRRNu12zuP8Ha18BHTPYznA4P6bO8xtlBuEb4w?e=rGTCF7>`_ which includes image reading, model loading, model prediction, and output drawing.

After downloading, extract the folder. If you are using a Docker Image, ensure the files are placed under /home/appuser/workdir, which is the local directory when the container starts.

Then, run the following command to execute the Python script and make predictions using the provided example model and image. The results will be output in the same folder as `output_with_masks_classes_scores.png`.

.. code-block:: python

    python3 example.py

You can change the file paths in `example.py` to use different images and deep learning models.

.. code-block:: python

    MODEL_ZIP = 'kp1.zip'
    IMG_PATH = 'kp.png'

You can also start with a new Python file. First, you need to import the `daoai_vision` library, which is our DaoAI World Python SDK.

.. code-block:: python

    import daoai_vision as dv

The following libraries may also be helpful:

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


Loading a Model

.. code-block:: python

    MODEL_ZIP = 'kp_fast_model.zip'  # DaoAI world model file path
    DEVICE = 'gpu'  # Hardware option: cpu or gpu. Using cpu will significantly slow down the model prediction.
    model = dv.get_model(model_zip=MODEL_ZIP, device=DEVICE)

Loading an Image and Running Model Prediction

.. code-block:: python

    IMG_PATH = 'test_image.png'  # Path to the image being read
    results = model.infer(IMG_PATH)  # Call the model prediction function

Prediction Results

.. code-block:: python

    boxes     = results.boxes     # [N][x1, y1, x2, y2]: bounding box formed by two points
    classes   = results.classes   # [N] Model class index values
    labels    = results.labels    # [N] Model class strings; you can use labels[classes[i]] to access the corresponding class string
    scores    = results.scores    # [N] Confidence score of the prediction
    masks     = results.masks     # [N][H][W] Model mask pixels
    keypoints = results.keypoints # [N][x, y, score] Model keypoints
    sem_masks = results.sem_seg   # Pixel masks

You can use these results as needed.

Drawing Bounding Boxes

.. code-block:: python

    # Draw bounding boxes
    if(boxes is not None):
        for box in boxes:
            cv2.rectangle(img, (int(box[0]), int(box[1])), (int(box[2]), int(box[3])), (0, 255, 0), 2)

Drawing Keypoints

.. code-block:: python

    if keypoints is not None:
        for obj in keypoints:
            for kp in obj:
                if float(kp[2]) > 0.5:
                    cv2.circle(img, (int(kp[0]), int(kp[1])), 2, (0, 0, 255), 2)

Drawing Masks, Classes, and Scores

.. code-block:: python

    # Draw masks, classes, and scores
    for i in range(len(boxes)):
        if masks is not None:
            mask = masks[i]

            # Resize the mask to image size
            mask_resized = cv2.resize(mask.astype(np.uint8), (img.shape[1], img.shape[0]))
            mask_rgb = np.zeros_like(img)
            mask_rgb[mask_resized > 0.5] = (255, 0, 255)  # Set the mask color
            # Draw mask
            img = cv2.addWeighted(img, 1, mask_rgb, 0.5, 0)

        cls = classes[i]
        score = scores[i]
        label = labels[cls]

        # Get current bounding box coordinates
        bbox = boxes[i]
        x1, y1, x2, y2 = int(bbox[0]), int(bbox[1]), int(bbox[2]), int(bbox[3])

        # Draw class and score
        text = f'{cls} {label}: {score:.2f}'
        cv2.putText(img, text, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 1)

Using the Example Code Prediction Results:

.. image:: images/output_with_masks_classes_scores.png
    :scale: 40%
