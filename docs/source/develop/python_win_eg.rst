Python Windows 代码示例
-----------------------------------

您可以使用我们给的 `Python示例代码 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/Elcb0srODHNGpDZYQu58mZsBeoD1173XVKj0YIvUalUGPA?e=OoBkUN>`_ 里面包含了图片的读取，模型的读取，以及模型的预测和输出。

您需要有效的DaoAI 许可证才可以运行，如果您没有许可证，请参考:ref:`DLSDK显示License Check Fail` 。

您需要将您的licensemanger_cli.exe 同license.lic文件放在python脚本同目录下。

然后运行以下命令就可以运行python脚本

.. code-block:: python

    python example.py

您可以通过更改example.py中的文件读取路径来使用不同的图片和深度学习模型。

.. code-block:: python

    model_path = "./kp1.zip"
    image_path = "./kp1.png"

您也可以从一个新的python文件开始，那么首先需要导入 相关的dll, 然后导入 dlsdk 库，也就是我们的DaoAI World Python Windows SDK

.. code-block:: python

    import os
    import sys
    systemDir = sys.prefix # System enviornment variable should point to DaoAISystem path.
    os.add_dll_directory(systemDir)
    import dlsdk.dlsdk as dlsdk

以下的库可能也会对您有帮助

.. code-block:: python

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt


初始化，读取模型

.. code-block:: python

    #初始化模型
    dlsdk.initialize()
    model = dlsdk.Model()

    model_path = "./kp1.zip"
    #读取模型
    model.load(model_path, device=dlsdk.DeviceType.GPU)

读取图片，这一步可以使用 opencv 来读取， 如果您没有 安装，您可以运行 ``pip install python-opencv`` 来进行安装

.. code-block:: python

    image_path = "./kp1.png" #读取的图片路径
    img = cv2.imread(image_path)

    daoai_image = dlsdk.Image.from_numpy(img, dlsdk.Image.Type.BGR) #创建 DaoAI Image 

模型预测，并输出结果为Json文件

.. code-block:: python

    assert isinstance(daoai_image, dlsdk.Image) #模型完整性检查
    prediction = model.inference(daoai_image) #模型预测

    with open("output.json", "w") as f:
        f.write(prediction.toJSONString()) # 结果输出

