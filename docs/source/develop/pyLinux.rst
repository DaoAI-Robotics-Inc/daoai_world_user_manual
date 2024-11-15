.. Python Linux/Jetson 环境配置
.. ---------------------------------

.. Python Linux/Jetson API 只支持在linux系统中使用，并分为 linux 机器，和Jetson 机器两种。

.. 如果您想在Windows环境中使用， 那么请参考 :ref:`1. 使用Docker Image` 配置Docker 虚拟环境。

.. 如果您使用的是linux环境，可以跳过 1. 并参考 :ref:`2. 安装DaoAI Python Linux/Jetson API Wheel` 来直接使用

.. 1. 使用Docker Image
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. 需要先安装Docker。

.. 安装完成后, 运行Docker 然后打开命令栏, 输入并运行以下命令 来下载Docker Container。注意 这一步需要预留约60G的磁盘空间。

.. Linux 机器的Docker环境映像 可以使用以下命令

.. .. code-block::

..     docker pull daoairobotics/daoai_vision 

.. 或者，如果您是Jetson 机器，可以使用以下命令来下载 Jetson机器的 Docker环境映像 

.. .. code-block::

..     docker pull daoairobotics/daoai_vision:jetson

.. .. image:: images/pull_docker.png
..     :scale: 100%
        
.. 下载完毕后，运行以下命令运行Docker Image 并进入虚拟映像的linux环境

.. .. code-block::

..     docker run -it --gpus=all -v <本地路径>:/home/appuser/workdir daoairobotics/daoai_vision

.. 其中

.. 1. <本地路径> 是您本地的路径，请替换为您实际的路径。
.. 2. /home/appuser/workdir 是Docker容器内的虚拟路径，该路径会包含第一步中指定路径的文件和文件夹。
.. 3. daoairobotics/daoai_vision 是Docker镜像的名称。

.. 输入命令后，如下图，您将以appuser登入Docker虚拟映像

..     .. image:: images/pscmd.png
..         :scale: 100%

.. 接下来请参考 :ref:`3. 激活您的Linux/Jetson SDK`

.. 2. 安装DaoAI Python Linux/Jetson SDK Wheel
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. 根据您的机器从 `下载中心 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhSzNT1zD61Pv8MbPMr_PM8BLRSiHwbywBsRHAEY5ILSiQ?e=DzrN2M>`_ 下载  

.. - 如果您使用的是 **Linux 机器**: 请下载 2.24.7.0版本的 ``Linux_wheels`` 目录下的.whl文件, 或

.. - 如果您使用的是 **Jetson 机器**: 请下载 2.24.7.0版本的 ``Jetson_wheels`` 目录下的.whl文件

.. Python Linux/Jetson SDK wheel 只支持linux环境，如果您使用的是Windows系统，那么请使用linux虚拟机，或者Docker Image。

.. Python Linux/Jetson SDK 支持的模型有：

.. .. list-table::
..    :header-rows: 1

..    * - 模型类型
..      - 快速模式
..      - 准确模式
..      - 旋转准确模式
..    * - 实例分割检测
..      - √  
..      - √  
..      - x 
..    * - 关键点检测
..      - √  
..      - √  
..      - x  
..    * - 异常检测
..      - x
..      - x  
..      - 
..    * - 分类检测
..      - √   
..      - √  
..      - 
..    * - 目标检测
..      - √  
..      - √  
..      - 
..    * - 语义分割
..      - √   
..      -  
..      - 
..    * - OCR
..      - x  
..      - 
..      - 


.. 2.a Linux 安装
.. ^^^^^^^^^^^^^^^^^^^

.. Linux 需要首先安装 **Python 3.10** 并且安装 **Nvidia Cuda Toolkit**

.. 如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. .. code-block::

..     python3 --version


.. - **默认安装方式** ：daoai_vision 提供了二进制的安装包（.whl 文件），方便用户在自己的环境中进行安装。安装时，.whl 文件会自动安装运行库所需的依赖，但并不会自动安装深度学习框架（如 PyTorch、ONNX、OpenVINO），这样用户可以选择自己需要的版本。

..     .. code-block::

..         pip install <Wheel-File>.whl

..     在此安装方式下，所有支持包都会被安装，用户可以直接运行本地推理。如果需要特定版本的重要依赖库，例如 PyTorch 或 ONNX，建议在安装后自行安装这些库。

.. - **深度学习库安装** ：如果需要使用本地 Python 进行推理，并希望安装所有必要的深度学习库，请使用以下命令：

..     .. code-block::

..         pip install <Wheel-File>.whl[dl]

..     该命令会同时安装推理所需的深度学习库。


.. 然后您的DaoAI Python Linux/Jetson SDK 模组就安装完毕了

.. 接下来请参考 :ref:`3. 激活您的Linux/Jetson SDK` 

.. 2.b Jetson 安装
.. ^^^^^^^^^^^^^^^^^^^

.. Linux 需要首先安装 **Python 3.8** 并且使用 venv 虚拟环境来运行

.. 如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. .. code-block::

..     python3 --version

.. 在工作路径下，使用以下命令来创建一个 venv 虚拟环境

.. .. code-block::

..     python3 -m venv <虚拟环境名称>
..     source <虚拟环境名称>/bin/activate


.. 然后使用以下命令安装wheel文件，使用时确保terminal的当前路径下包含whl文件。

.. - **默认安装方式** ：daoai_vision 提供了二进制的安装包（.whl 文件），方便用户在自己的环境中进行安装。安装时，.whl 文件会自动安装运行库所需的依赖，但并不会自动安装深度学习框架（如 PyTorch、ONNX、OpenVINO），这样用户可以选择自己需要的版本。

..     .. code-block::

..         pip install <Wheel-File>.whl

..     在此安装方式下，所有支持包都会被安装，用户可以直接运行本地推理。如果需要特定版本的重要依赖库，例如 PyTorch 或 ONNX，建议在安装后自行安装这些库。

.. - **深度学习库安装** ：如果需要使用本地 Python 进行推理，并希望安装所有必要的深度学习库，请使用以下命令：

..     .. code-block::

..         pip install <Wheel-File>.whl[dl]

..     该命令会同时安装推理所需的深度学习库。


.. 请注意您需要遵循官方的网址下载正确版本的 torchvision https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048 

.. 也可以参考以下命令，以下命令安装的是 torchvision 0.16.1

.. .. code-block::

..     sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
..     git clone --branch v0.16.1 https://github.com/pytorch/vision torchvision

..     cd torchvision
..     export BUILD_VERSION=0.16.1
..     python3 setup.py install
..     cd ../ 


.. 然后您的DaoAI Python Linux/Jetson SDK 模组就安装完毕了

.. 接下来请参考 :ref:`3. 激活您的Linux/Jetson SDK` 

.. 3. 激活您的Linux/Jetson SDK
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. 接下来您需要在终端中运行以下命令来激活您的许可证，如果您没有激活码 和机器许可证文件，请参考 :ref:`软件许可证`

.. .. code-block::

..     daoai_vision activate --license ABCDEF-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX --machine <machine-file-path>

.. 该命令会验证您的许可证文件，并缓存30天，之后需要重新运行命令。

.. .. note::

..     重新运行时，需要添加 -r flag 来清除之前的许可证缓存，如果您使用了无效的许可证，再次尝试激活时 也需要使用 -r flag

..     .. code-block::
..         daoai_vision activate -r --license ABCDEF-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX --machine <machine-file-path>


.. 激活成功后您就可以正常使用Python SDK了， 您可以使用import daoai_vision 语句来导入模组。 

.. .. code-block:: python

..     import daoai_vision as dv

.. 4. Python Linux/Jetson SDK 的使用
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. daoai_vision 支持多种推理部署方式，包括本地部署和远程托管部署。根据不同的需求，可以选择适合的部署方式来运行模型推理。主要有三种部署方式：

.. 1. 自托管部署
..     自托管部署是在用户自己的硬件上运行模型推理，主要有两种方式： **原生Python部署** 和 **Docker 服务器部署** 。

..     **原生 Python 部署**

..     使用原生 Python 在本地硬件上运行推理。可以通过以下代码加载模型并进行推理：

..     .. code-block:: python
        
..         import daoai_vision as dv
..         MODEL_ZIP = 'path/to/downloaded/zip'  # 模型文件 (.zip / .dwm)
..         DEVICE = 'gpu'                        # 可选设备: cpu / gpu 
..         IMG_PATH = 'path/to/image'            # 待推理的图像路径

..         model = dv.get_model(model_zip=MODEL_ZIP, device=DEVICE)
..         results = model.infer(IMG_PATH)

..     也可以通过命令行工具运行推理，使用如下命令：

..     .. code-block::

..         daoai_vision infer -i /path/to/image.png -m /path/to/model.zip --native-python

..     该命令会在终端打印推理结果的概要，并将结果保存为 JSON 文件，输出到与输入图像相同的目录。


..     **Docker 服务器部署**

..     使用 Docker 运行推理服务器，服务器可以在后台运行，并接受推理请求。

..     1. 启动服务器：

..         .. code-block::

..             daoai_vision server enable

..     2. 运行推理请求：
        
..         可以通过命令行工具发送推理请求：

..         .. code-block::

..             daoai_vision infer -i /path/to/image.png -m /path/to/model.zip

..         也可以在 Python 中设置 server=True 来发送推理请求：

..         .. code-block:: python

..             import daoai_vision as dv
..             model = dv.get_model(model_zip=MODEL_ZIP, server=True)
..             results = model.infer(IMG_PATH)

.. 2. 托管部署
..     托管部署允许将推理请求发送到 DaoAI World 的远程推理服务器。此时，需要指定远程服务器的 URL。

..     通过命令行发送请求：
    
..     .. code-block::

..         daoai_vision infer -i /path/to/image.jpg -m /path/to/model.zip -url http://remote-server.com:PORT


..     在 Python 中使用托管推理：

..     .. code-block:: python

..         import daoai_vision as dv
..         model = dv.get_model(model_zip=MODEL_ZIP, server=True)
..         results = model.infer(IMG_PATH, url='http://remote-server.com:PORT')

