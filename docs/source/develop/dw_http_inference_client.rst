DaoAI World服务器推理服务
---------------------------------

在能够使用互联网的情况下，可以使用HTTP请求发送到DaoAI World服务器进行推理。以下是实例代码：

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


训练好的模型
~~~~~~~~~~~~~~~~~~~~~~~~~

您可以使用任意一个您训练好的模型进行推理。您需要引用 ``InferenceHTTPClient`` 库，然后提供您的账户( ``API Key`` )和模型的版本ID( ``trained_model_uid`` )，建立其客户端，再进行推理。

 - 使用您账户中模型的示例代码：

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

您可以在训练好的项目中，点击 :ref:`模型部署` 查看该模型的 ``API Key`` 和 ``trained_model_uid``。

.. note::
    不同版本的模型拥有不同的 ``trained_model_uid``，请仔细确认。

.. warning::
    API Key 是您账户远程访问 DaoAI World 的凭证。为了您的数据安全，请妥善保管您的 API Key，以免被他人盗用。

推理返回的结果是一个 **字典**，其中含有：
 
 - **"inference_time"**
 - **"inference_device"**
 - **"result"**

返回的结果都是来自 requests 库的 **JSON** 对象，包含相同的常用字段，例如掩码(masks)、边框(boxes)、分数(scores)等。

预训练模型
~~~~~~~~~~~~~~~~~~~~~~~~~

您也可以使用 DaoAI World 提供的预训练模型，其中包括： ``OCR`` 和 ``auto_mask`` ，以及 ``mask_predictor`` 模型。

OCR
**********

OCR 模型会使用DaoAI的预训练模型对图片中的文字进行识别，并返回结果

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

返回的结果都是来自 requests 库的 **JSON** 对象，包含相同的常用字段，例如掩码(masks)、边框(boxes)、分数(scores)等。

auto_mask
**********

auto_mask 模型是DaoAI World服务器提供的 **全局** 智能分割图片的模型，它可以接受一些参数
  
  - points_per_side:
      这个参数控制了生成点提示的密度。数值越大，生成的点就越密集，覆盖图像的范围就越广，这有助于模型更精确地捕捉到物体的细节。数值越小，生成的点就越稀疏，模型可能会错过一些小的或复杂的物体。
  
  - box_nms_thresh:
      NMS (Non-Maximum Suppression) 是一个常用的后处理步骤，用于去除冗余的检测框。box_nms_thresh 这个阈值决定了两个检测框之间的IoU（Intersection over Union）必须小于多少才能被认为是不同的物体。数值越大，NMS 就会更加严格，去除的冗余框越多。数值越小，NMS 就越宽松，保留的框越多，但可能会引入更多的误检。
  
  - pred_iou_thresh:
      这个参数用于过滤掉预测的分割掩码中面积过小的区域。pred_iou_thresh 表示预测的掩码与原始输入图像的IoU。数值越大，过滤掉的掩码就越多，只保留与输入图像重叠较大的掩码。数值越小，保留的掩码就越多，但可能会包含一些噪声。
  
  - min_mask_region_area:
      这个参数直接控制了最小掩码区域的面积。数值越大，过滤掉的掩码就越多，只保留面积较大的掩码。数值越小，保留的掩码就越多，但可能会包含一些非常小的、不相关的区域。
  
  - point_grids=None,
      这个参数通常用于指定生成点提示的网格。None 表示随机生成点。如果指定了特定的网格，则会按照网格的规则生成点，这有助于在特定区域生成更密集的点。

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
        points_per_side= 48,
        box_nms_thresh= 0.7,
        pred_iou_thresh=0.7,
        min_mask_region_area=100,
        point_grids=None,
    )

返回结果可视化图片保存示例代码

保存全部结果为图片

.. code-block:: python

    import base64
    from io import BytesIO
    from PIL import Image

    #save sam image
    #Decode the base64 image
    img_data = base64.b64decode(result['image'])
    # Open the decoded image data with Pillow
    img = Image.open(BytesIO(img_data))
    # Save the image as PNG
    img.save('output.png', 'PNG')

获取全部结果其中的一个掩码并显示

.. code-block:: python

    import matplotlib.pyplot as plt
    from pycocotools import mask

    # access single mask
    rle_data = result['masks'][0]['segmentation']
    # decode coco RLE(Run-length encoding)
    binary_mask = mask.decode(rle_data)
    # display
    plt.imshow(binary_mask, cmap="gray")
    plt.axis("off")
    plt.show()

mask_predictor
******************

auto_mask 模型是DaoAI World服务器提供的 **引导** 智能分割图片的模型，这个比较类似于标注时的智能标注工具，您可以输入引导的点，或者框，模型会返回您指示区域中的掩码。
它可以接受一些参数

  - point_coords:
    这个参数表示点提示的坐标。在图像上指定一些点，告诉模型你希望它关注这些区域。这些点可以是图像中你感兴趣的物体的中心、边界点等。通常是一个二维的NumPy数组，每一行代表一个点的坐标 (x, y)。
    
  - box:
    这个参数表示一个边界框。用一个矩形框来大致框定你感兴趣的区域。这可以帮助模型更快地聚焦到目标区域。通常是一个列表或数组，包含四个元素：[x1, y1, x2, y2]，分别表示框的左上角和右下角的坐标。

  - point_labels:
     这个参数表示点提示的标签。为每个点提示分配一个标签，告诉模型这个点属于哪个类别。这在多类别分割任务中非常有用。通常是一个与 point_coords 形状相同的数组，每个元素表示对应点的标签，告诉模型这个点属于哪个类别。这在多类别分割任务中非常有用。

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
    x_min, y_min, x_max, y_max = 400,400,600,600  #replace with your coordinates
    box = [x_min, y_min, x_max, y_max]
    result = CLIENT.infer(file_path, pretrained_model_type="mask_predictor",
        point_coords = None,
        box = box,
        point_labels = None,
    )

.. code-block:: python

    # use Point
    point_coords = np.array([[100, 50], [200, 150]]) #replace with your coordinates
    point_labels = np.array([0, 1]) #optional give the label of the cooridnates
    result = CLIENT.infer(file_path, pretrained_model_type="mask_predictor",
      point_coords = point_coords,
      box = None,
      point_labels = point_labels,
    )

返回结果可视化图片保存示例代码

.. code-block:: python

    import cv2
    from pycocotools import mask

    image = cv2.imread(file_path)
    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    # 将 coco RLE(Run-length encoding) 解码为二值掩膜
    binary_mask = mask.decode(result)
    
    # 找到掩膜的最小边界框
    x_coords, y_coords = np.where(binary_mask > 0)
    if len(x_coords) == 0 or len(y_coords) == 0:
        print("No mask detected.")
        continue
    x_min, x_max = np.min(x_coords), np.max(x_coords)
    y_min, y_max = np.min(y_coords), np.max(y_coords)
    
    # 创建空白透明背景，并按mask裁剪
    masked_image = np.zeros_like(image_rgb, dtype=np.uint8)
    masked_image = cv2.bitwise_and(image_rgb, image_rgb, mask=binary_mask)

    # 使用边界框裁剪图像，去除黑色边框
    cropped_image = masked_image[x_min:x_max+1, y_min:y_max+1]

    # 保存裁剪后的图片
    current_time = datetime.now().strftime('%Y%m%d_%H%M%S')
    cut_out_name = f"cut_out_{current_time}_{i}.png"
    output_folder = "./"
    cut_out_path = os.path.join(output_folder, cut_out_name)
    cv2.imwrite(cut_out_path, cv2.cvtColor(cropped_image, cv2.COLOR_RGB2BGR))

    print("Image saved to:", cut_out_path)