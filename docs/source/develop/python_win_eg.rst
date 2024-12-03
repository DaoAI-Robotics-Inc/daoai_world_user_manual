Python Windows 代码示例
-----------------------------------

您可以使用我们给的 `Python示例代码 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/Elcb0srODHNGpDZYQu58mZsBeoD1173XVKj0YIvUalUGPA?e=OoBkUN>`_ 里面包含了图片的读取，模型的读取，以及模型的预测和输出。

.. contents::
    :local:

    
您需要有效的DaoAI 许可证才可以运行，如果您没有许可证，请参考 :ref:`软件许可证` 。

然后运行以下命令就可以运行python脚本

.. code-block:: python

    python example.py

您可以通过更改example.py中的文件读取路径来使用不同的图片和深度学习模型。

.. code-block:: python

    model_path = "./model.dwm"
    image_path = "./image.png"


引入库
~~~~~~~~~~~~~~~~

您也可以从一个新的python文件开始，那么首先需要导入 dlsdk 库，也就是我们的DaoAI World Python Windows SDK


.. code-block:: python

    import os
    import sys
    import dlsdk.dlsdk as dlsdk

以下的库可能也会对您有帮助

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


加载深度学习模型
~~~~~~~~~~~~~~~~

.. code-block:: python

    #初始化模型
    dlsdk.initialize()
    model_path = "./model.dwm"
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)


注意，这里每一个检测任务都有对应的对象：

.. code-block:: python

    #实例分割
    model = dlsdk.InstanceSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    #关键点检测
    model = dlsdk.KeypointDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #图像分类
    model = dlsdk.Classification(model_path, device=dlsdk.DeviceType.GPU)
    
    #目标检测
    model = dlsdk.ObjectDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #异常检测(适用于.6版本以前的类型名称，.7后更名为 非监督缺陷检测)
    model = dlsdk.AnomalyDetection(model_path, device=dlsdk.DeviceType.GPU)
    
    #语义分割(适用于.6版本以前的类型名称，.7后更名为 监督缺陷检测)
    model = dlsdk.SemanticSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    #非监督缺陷检测
    model = dlsdk.UnsupervisedDefectSegmentation(model_path, device=dlsdk.DeviceType.GPU)
    
    #监督缺陷检测
    model = dlsdk.SupervisedDefectSegmentation(model_path, device=dlsdk.DeviceType.GPU)

    #OCR
    model = dlsdk.OCR(model_path, device=dlsdk.DeviceType.GPU)

    #定位模型 (只在工业版支持)
    model = dlsdk.Positioning(model_path, device=dlsdk.DeviceType.GPU)

    #漏错装检测 (只在工业版支持)
    model = dlsdk.PresenceChecking(model_path, device=dlsdk.DeviceType.GPU)


读取图片
~~~~~~~~~~~~~~~~


读取图片，这一步可以使用 opencv 来读取， 如果您没有 安装，您可以运行 ``pip install python-opencv`` 来进行安装

.. code-block:: python

    image_path = "./kp1.png" #读取的图片路径
    img = cv2.imread(image_path)

    daoai_image = dlsdk.Image.from_numpy(img, dlsdk.Image.Type.BGR) #创建 DaoAI Image 



使用深度学习模型进行预测
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


模型预测，并输出结果为Json文件

.. code-block:: python

    assert isinstance(daoai_image, dlsdk.Image)
    prediction = model.inference(daoai_image,{dlsdk.PostProcessType.CONFIDENCE_THRESHOLD: 0.95})

    with open("output.json", "w") as f: 
        f.write(prediction.toJSONString()) #输出常规json结果

    with open("outputAnnotation.json", "w") as f:
        f.write(prediction.toAnnotationJSONString()) #输出标注格式的json结果

后处理
***********************

模型的预测 可以接受后处理参数：

    - dlsdk.PostProcessType.CONFIDENCE_THRESHOLD 
        
        置信度阈值，会过滤掉结果中置信度低于设定值的结果
        
    - dlsdk.PostProcessType.IOU_THRESHOLD 
        
        IOU阈值，会过滤掉结果中IOU低于设定值的结果

    - dlsdk.PostProcessType.SENSITIVITY_THRESHOLD 
        
        非监督缺陷分割（异常检测）模型中使用敏感度，控制模型对于缺陷的敏感度，越高则模型会检测出越多的缺陷，但是容易误检

.. code-block:: python

    prediction = model.inference(daoai_image,{dlsdk.PostProcessType.CONFIDENCE_THRESHOLD: 0.95, dlsdk.PostProcessType.IOU_THRESHOLD: 0.5})



您也可以通过其它方法来获取结果信息

.. code-block:: python

    print(prediction.boxes)
    print(prediction.class_ids) 
    print(prediction.class_labels)
    
获取预测结果
~~~~~~~~~~~~~~~~

Box（边界框）
``````````````````````````````

    Box 是模型预测结果中的边界框，可通过以下方式获取：

.. code-block:: python

    prediction = model.inference(daoai_image)

    prediction.boxes[0].x1();  // 左上角 x 坐标
    prediction.boxes[0].y1();  // 左上角 y 坐标
    prediction.boxes[0].x2();  // 右下角 x 坐标
    prediction.boxes[0].y2();  // 右下角 y 坐标

Mask（掩码）
``````````````````````````````

Mask 是预测结果中目标物体的外轮廓信息，可以通过以下方式提取多边形区域的顶点：

.. code-block:: python

    prediction = model.inference(daoai_image)

    prediction.masks[0].toPolygons()[0].points[0].x  // 顶点的 X 坐标
    prediction.masks[0].toPolygons()[0].points[0].y  // 顶点的 Y 坐标


- **`masks[0]`**：提取第一个目标物体的掩码。  
- **`toPolygons()`**：将掩码转换为多边形外轮廓。  
- **`points[0]`**：获取多边形的第一个顶点坐标。  

通过顺序连接多边形的所有顶点，可以形成完整的轮廓区域，用于目标检测、区域分析等应用场景。

可视化输出
~~~~~~~~~~~~

生成并保存预测结果与原图叠加的可视化图像：

.. code-block:: python

    result = dlsdk.visualize(daoai_image, prediction)

    result.save("output.png")

