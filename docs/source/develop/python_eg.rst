Python Linux/Jetson Code Example
--------------------------------------------------

You can use the provided `Python example code <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/Ed6ajuVWvRRNu12zuP8Ha18BHTPYznA4P6bO8xtlBuEb4w?e=rGTCF7>`_ ,which includes image reading, model loading, prediction, and output visualization.

After downloading and extracting the folder, if you are using Docker Image, make sure the files are placed in ``/home/appuser/workdir`` , which is the local directory at startup.

Then, run the following command to execute the Python script and perform model prediction with the provided example model and image. 
The results will be output to ``output_with_masks_classes_scores.png`` in the same folder.

.. code-block:: python

    python3 example.py

You can modify the file paths in ``example.py`` to use different images and deep learning models.

.. code-block:: python

    MODEL_ZIP = 'kp1.zip'
    IMG_PATH = 'kp.png'

Start by importing the DaoAI World Python SDK and other useful libraries:

.. code-block:: python

    import daoai_vision as dv

The following libraries can also be helpful

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


Load DaoAI Models

.. code-block:: python

    MODEL_ZIP = 'kp_fast_model.zip'  # Path to the DaoAI World model file
    DEVICE = 'gpu'  # Hardware option: 'cpu' or 'gpu'. Using 'cpu' will significantly slow down prediction.
    model = dv.get_model(model_zip=MODEL_ZIP, device=DEVICE)

Read the image, and run inference

.. code-block:: python

    IMG_PATH = 'test_image.png'  # Path to the image
    results = model.infer(IMG_PATH)  # Perform model inference


Inference result

.. code-block:: python

    boxes = results.boxes       # [N][x1, y1, x2, y2]: bounding boxes
    classes = results.classes   # [N]: model class index values
    labels = results.labels    # [N]: model class labels (use labels[classes[i]] for class string)
    scores = results.scores     # [N]: prediction confidence scores
    masks = results.masks       # [N][H][W]: model mask pixels
    keypoints = results.keypoints  # [N][x, y, score]: model keypoints
    sem_masks = results.sem_seg # Pixel masks

you can use these results as the following

Draw bounding box

.. code-block:: python

    # Draw bounding boxes
    if(boxes is not None):
        for box in boxes:
            cv2.rectangle(img, (int(box[0]), int(box[1])), (int(box[2]), int(box[3])), (0, 255, 0), 2)

Draw keypoints

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

            # Resize mask to image size
            mask_resized = cv2.resize(mask.astype(np.uint8), (img.shape[1], img.shape[0]))
            mask_rgb = np.zeros_like(img)
            mask_rgb[mask_resized > 0.5] = (255, 0, 255)  # Set mask color
            # Draw mask
            img = cv2.addWeighted(img, 1, mask_rgb, 0.5, 0)

        cls = classes[i]
        score = scores[i]
        label = labels[cls]

        # Get coordinates of current bounding box
        bbox = boxes[i]
        x1, y1, x2, y2 = int(bbox[0]), int(bbox[1]), int(bbox[2]), int(bbox[3])

        # Draw class and score
        text = f'{cls} {label}: {score:.2f}'
        cv2.putText(img, text, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 255, 255), 1)



The prediction results are visualized and saved as 

.. image:: images/output_with_masks_classes_scores.png
    :scale: 40%
    
