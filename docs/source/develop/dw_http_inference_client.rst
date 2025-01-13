DaoAI World Cloud Inference Service
---------------------------------------------

When connected to the internet, you can send HTTP requests to the DaoAI World server for inference. Below is the example code:

.. code-block:: python

    # import the inference-sdk
    from inference_client import InferenceHTTPClient
    # initialize the client
    CLIENT = InferenceHTTPClient(
    api_endpoint="https://api.dev.daoai.ca",
    api_key="XXXXXXXXXXXXXXXXXXX"
    )

    # Infer with TRAINED MODEL
    result = CLIENT.infer("YOUR_IMAGE.jpg",
    trained_model_uid="XXXXXXXXXXXXXXXXXXX",
    )

    # Infer with a PRETRAINED MODEL (Eg, auto_segment)
    result = CLIENT.infer("YOUR_IMAGE.jpg",
    pretrained_model_type = 'auto_segment'
    )


Trained Models
~~~~~~~~~~~~~~~~~~~~~~~~~

You can use any of the trained models for inference. You need to import the ``InferenceHTTPClient`` library, provide your account's ``API Key`` and the model's version ID (``trained_model_uid``), initialize the client, and then perform inference.

- Example code for using a model from your account:

.. code-block:: python

    # import the inference-sdk
    from inference_client import InferenceHTTPClient

    # initialize the client
    CLIENT = InferenceHTTPClient(
    api_endpoint="https://api.dev.daoai.ca",
    api_key="XXXXXXXXXXXXXXXXXXX"
    )

    # Infer with TRAINED MODEL
    result = CLIENT.infer("YOUR_IMAGE.jpg",
    trained_model_uid="XXXXXXXXXXXXXXXXXXX",
    )

You can find the model's ``API Key`` and ``trained_model_uid`` by clicking :ref:`Model Deployment` in your trained project.

.. note::
    Different versions of the model have different ``trained_model_uid``, so be sure to check carefully.

.. warning::
    The API Key is your credential for remote access to DaoAI World. To ensure your data security, please store your API Key safely to prevent unauthorized use.

The inference result returned is a **dictionary**, which contains:

- **"inference_time"**
- **"inference_device"**
- **"result"**

The returned result is a **JSON** object from the requests library, containing common fields such as masks, boxes, scores, etc.

Pretrained Models
~~~~~~~~~~~~~~~~~~~~~~~~~

You can also use the pretrained models provided by DaoAI World, which include: ``OCR``, ``auto_mask``, and ``mask_predictor``.

OCR
**********

The OCR model uses DaoAI's pretrained model to recognize text in images and return the results.

.. code-block:: python

    # import the inference-sdk
    from inference_client import InferenceHTTPClient
    # initialize the client
    CLIENT = InferenceHTTPClient(
    api_endpoint="https://api.dev.daoai.ca",
    api_key="XXXXXXXXXXXXXXXXXXX"
    )

    # Infer with a PRETRAINED MODEL (Eg, auto_segment)
    result = CLIENT.infer("YOUR_IMAGE.jpg", pretrained_model_type="ocr")

The returned result is a **JSON** object from the requests library, containing common fields such as masks, boxes, scores, etc.

auto_mask
**********

The auto_mask model is a **global** intelligent segmentation model provided by DaoAI World server. It accepts several parameters:

- points_per_side:
    This parameter controls the density of generated point hints. A larger value means denser points covering a wider area, helping the model capture more details. A smaller value may miss small or complex objects.

- box_nms_thresh:
    NMS (Non-Maximum Suppression) is a post-processing step to remove redundant detection boxes. The threshold ``box_nms_thresh`` determines the minimum IoU (Intersection over Union) required between two boxes to be considered different objects. A larger value makes NMS stricter, removing more redundant boxes. A smaller value keeps more boxes but may introduce more false positives.

- pred_iou_thresh:
    This parameter filters out small predicted segmentation regions. It represents the IoU between the predicted mask and the original input image. A higher value removes smaller masks, retaining only those with higher overlap. A lower value keeps more masks, but may include noise.

- min_mask_region_area:
    This directly controls the minimum area of the mask region. A larger value removes smaller masks, keeping larger ones.

- point_grids=None:
    This parameter specifies the grid for generating point hints. If set to None, the points are randomly generated. If a specific grid is provided, points are generated based on the grid, helping to generate denser points in certain areas.

.. code-block:: python

    # import the inference-sdk
    from inference_client import InferenceHTTPClient
    # initialize the client
    CLIENT = InferenceHTTPClient(
    api_endpoint="https://api.dev.daoai.ca",
    api_key="XXXXXXXXXXXXXXXXXXX"
    )

    # Infer with a PRETRAINED MODEL (Eg, auto_segment)
    result = CLIENT.infer("YOUR_IMAGE.jpg", pretrained_model_type="auto_mask",
        points_per_side=48,
        box_nms_thresh=0.7,
        pred_iou_thresh=0.7,
        min_mask_region_area=100,
        point_grids=None,
    )

Saving and visualizing inference results

Save the entire result as an image:

.. code-block:: python

    import base64
    from io import BytesIO
    from PIL import Image

    #save same image
    # Decode the base64 image
    img_data = base64.b64decode(result['image'])
    # Open the decoded image data with Pillow
    img = Image.open(BytesIO(img_data))
    # Save the image as PNG
    img.save('output.png', 'PNG')

Get one mask from the result and display:

.. code-block:: python

    import matplotlib.pyplot as plt
    from pycocotools import mask

    # access single mask
    rle_data = result['masks'][0]['segmentation']
    # decode coco RLE (Run-length encoding)
    binary_mask = mask.decode(rle_data)
    # display
    plt.imshow(binary_mask, cmap="gray")
    plt.axis("off")
    plt.show()

mask_predictor
******************

The mask_predictor model is a **guided** intelligent segmentation model provided by DaoAI World server, similar to smart labeling tools used during annotation. You can input guiding points or boxes, and the model will return the mask for the indicated region.
It accepts the following parameters:

  - **point_coords**:
    This parameter represents the coordinates of the guiding points. Specify some points on the image to tell the model which regions to focus on. These points can represent the centers or boundaries of the objects you're interested in. Typically, this is a 2D array where each row represents the coordinates (x, y) of a point.
    
  - **box**:
    This parameter represents a bounding box. It outlines the region of interest with a rectangle. This helps the model focus more quickly on the target area. Typically, this is a list or array with four elements: `[x1, y1, x2, y2]`, which represent the coordinates of the top-left and bottom-right corners of the box.

  - **point_labels**:
    This parameter represents the labels for the guiding points. Assign a label to each point to tell the model which category it belongs to. This is particularly useful in multi-class segmentation tasks. It is typically an array of the same length as `point_coords`, with each element representing the label of the corresponding point.

.. code-block:: python

    # import the inference-sdk
    from inference_client import InferenceHTTPClient
    # initialize the client
    CLIENT = InferenceHTTPClient(
    api_endpoint="https://api.dev.daoai.ca",
    api_key="XXXXXXXXXXXXXXXXXXX"
    )

.. code-block:: python

    # use Box
    x_min, y_min, x_max, y_max = 400, 400, 600, 600  # Replace with your coordinates
    box = [x_min, y_min, x_max, y_max]
    result = CLIENT.infer(file_path, pretrained_model_type="mask_predictor",
        point_coords=None,
        box=box,
        point_labels=None,
    )

.. code-block:: python

    # use Point
    point_coords = [[100, 50], [200, 150]]  # Replace with your coordinates
    point_labels = [0, 1]  # Assign the labels for the coordinates (0: background, 1: object)
    result = CLIENT.infer(file_path, pretrained_model_type="mask_predictor",
      point_coords=point_coords,
      box=None,
      point_labels=point_labels,
    )

Saving and visualizing inference results

.. code-block:: python

    import cv2
    from pycocotools import mask

    image = cv2.imread(file_path)
    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    # Decode coco RLE (Run-Length Encoding) into a binary mask
    binary_mask = mask.decode(result)
    
    # Find the minimum bounding box of the mask
    x_coords, y_coords = np.where(binary_mask > 0)
    if len(x_coords) == 0 or len(y_coords) == 0:
        print("No mask detected.")
        continue
    x_min, x_max = np.min(x_coords), np.max(x_coords)
    y_min, y_max = np.min(y_coords), np.max(y_coords)
    
    # Create an empty transparent background and crop according to the mask
    masked_image = np.zeros_like(image_rgb, dtype=np.uint8)
    masked_image = cv2.bitwise_and(image_rgb, image_rgb, mask=binary_mask)

    # Crop the image using the bounding box and remove the black border
    cropped_image = masked_image[x_min:x_max+1, y_min:y_max+1]

    # Save the cropped image
    current_time = datetime.now().strftime('%Y%m%d_%H%M%S')
    cut_out_name = f"cut_out_{current_time}_{i}.png"
    output_folder = "./"
    cut_out_path = os.path.join(output_folder, cut_out_name)
    cv2.imwrite(cut_out_path, cv2.cvtColor(cropped_image, cv2.COLOR_RGB2BGR))

    print("Image saved to:", cut_out_path)
