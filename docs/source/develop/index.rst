开发功能
=============

DW_SDK Windows安装包
-------------------------

SDK版本
***********

SDK 和 DaoAI World 一样，分为 **工业版** 和 **企业版**。

两个版本的区别在于， **工业版** 可以支持工业版特有的两种类型的模型： :ref:`漏错装检测` 和 :ref:`定位模型` 。

其他方面，两个版本并无不同，在模型的使用和性能上是一样的。

硬件需求
***************

DaoAI World SDK 支持CPU模式和GPU模式。尽管您没有GPU 也可以使用DaoAI World 深度学习模型使用CPU进行预测。

当使用GPU模式时，模型的运行时间会显著快于CPU模式。

使用GPU模式的最低需求为:

- 显卡: **Nvidia 1050Ti 显卡, 4GB 显存**

- 显卡驱动： **GeForce Game Ready Driver 驱动版本：552.22， 发布于 2024年 4月16日**


安装
***************

    首先需要下载 `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_
    Windows C++ C# SDK 目录下的 2.22.7.0 安装包zip
    
    请注意，安装包分为 **企业版** 和 **工业版** ，请下载您使用版本对应的SDK，以确保可以正常使用。

    或者从百度网盘里下载 https://pan.baidu.com/s/1gE2QuiVTaaMrVMAzVtRo2g?pwd=g4un 提取码: g4un 

    把下载的zip文件解压后，会看到3个文件，分别是： `daoai_world_sdk_2.0_setup.exe`, `daoai_world_sdk_2.0_setup-1.bin` 和 `daoai_world_sdk_2.0_setup-1.bin` ，双击运行其中的 `daoai_world_sdk_2.0_setup.exe` 执行文件开始安装。

    .. image:: images/dlsk_installer_unzip.png
            :scale: 80%

    .. note::

        DW_SDK 安装包需要磁盘中存在6.7GB以上的空间。

    - 打开安装程序，安装的路径为： ``C:\Program Files\DaoAI World SDK`` 。

    - 选择 ``创建桌面快捷方式``，以便直接管理SDK的软件许可证。

    .. image:: images/dlsk_installer_desktop_shortcut.png
            :scale: 80%   

    - 点击 ``安装``，安装包开始运行安装，请耐心等待。

    .. image:: images/dlsk_installer_install.png
            :scale: 80%   

    - 点击 ``完成``，结束安装。

    .. image:: images/dlsk_installer_done.png
            :scale: 80%   

软件许可证
***************

DW_SDK 需要拥有 `DaoAI` 官方授权的软件许可证才能使用，请联系您的支持工程师或者客户获取许可证。您需要为 `DaoAI` 的工作人员提供您电脑的信息：

    - 双击桌面 ``DW_SDK`` 快捷方式，打开许可证管理中心。

    .. image:: images/dlsk_installer_icon.png
            :scale: 80%   
    
    - 打开许可证管理中心，可以见到以下界面。

    .. image:: images/dlsk_installer_license_manager_gui.png
            :scale: 80%   

    - 点击 ``文件``，打开 ``电脑信息``。
    
    .. image:: images/dlsk_installer_machineid.png
            :scale: 80%  

    - 在这里，您可以查看到您电脑的名称以及机器码。点击 ``复制电脑ID``，可以把机器码复制发送至 `DaoAI` 的工作人员为您授权。
    
    .. image:: images/dlsk_installer_copy_id.png
            :scale: 80%  

在线授权（需联网）
~~~~~~~~~~~~~~~~

获得许可证后，您可以在界面上直接添加：

    - 点击 ``文件``，打开 ``添加软件许可证``。

    .. image:: images/dlsk_installer_add_online_license.png
        :scale: 80%  

    - 输入从 `DaoAI` 的工作人员获得的有效许可证，许可证为32位的数字和英文字母组成的激活码，复制粘贴到以下界面中。

    .. image:: images/dlsk_installer_add_online_license_1.png
        :scale: 80%  

    - 点击 ``查看``，验证许可证信息。

    .. image:: images/dlsk_installer_online_license_check.png
        :scale: 80%  

    - 管理器与服务器确认许可证已激活，可以正常使用。

    .. image:: images/dlsk_installer_online_license_check_good.png
        :scale: 80%  
    
    .. note::
        在线授权需要电脑连接互联网。

离线许可证
~~~~~~~~~~~~

由于环境限制，部分用户无法将设备联网，`DaoAI` 的工作人员会为您提供离线许可证。获得许可证后，您可以在界面上直接添加：

    - 点击 ``文件``，打开 ``导入离线许可证文件``。

    .. image:: images/dlsk_installer_add_offline_license.png
        :scale: 80%  

    - 选择 `DaoAI` 的工作人员为您提供的离线许可证文件。

    .. image:: images/dlsk_installer_select_offline_license.png
        :scale: 80%  

    
    - 管理器确认许可证已激活，可以正常使用。

    .. image:: images/dlsk_installer_offline_license_check.png
        :scale: 80%  
    
系统环境变量
***************

DW_SDK 安装包会自动建立 DW_SDK 所需的系统环境变量： ``DWSDK_PATH`` 。在使用 DW_SDK 时可以直接引用此变量即可。

    .. image:: images/dlsk_installer_dwsdk_path.png
            :scale: 80%  


    **C++** 和 **C#** 的SDK安装包在安装后的安装目录下的 DWSDK 文件夹内包含有SDK以及SDK的示例项目。

        .. image:: images/install_folder.png
            :scale: 100%

    使用Visual Studio打开DLSDK Example.sln项目。

        .. image:: images/example_path.png
            :scale: 100%

        .. image:: images/vs.png
            :scale: 60%

    项目分为C++项目，和C#项目，右键点击properties, 然后选择启动项目，来选择运行C++或者C#项目。

        .. image:: images/start_up.png
            :scale: 70%

    然后选择启动设置为release x64, 然后点击Local Windows Debugger 就可以运行项目了。

        .. image:: images/run_0.png
            :scale: 60%
            
        .. image:: images/run_1.png
            :scale: 60%

    使用DW_SDK需要有效的使用许可证，详情请见 :ref:`软件许可证`

C++ 项目配置
***********************

    C++的示例项目中的环节已经配置好了，如果您需要创建一个 **自定义项目** ，或者 **从一个空项目开始** ，则需要进行以下的配置。

    右键点击c++的项目，然后打开属性。
        
        .. image:: images/cpp_env1.png
            :scale: 70%

    在属性中选择使用C++17 

        .. image:: images/cpp17.png
            :scale: 80%

    打开C++, 在General菜单里的Additional Include Directories中添加 DW_SDK 根目录下的 include 文件夹路径。

        .. image:: images/cpp_env2.png
            :scale: 80%

    打开Linker, 在General菜单里的Additional Library Directories中添加 DW_SDK 根目录下的 bin 文件夹路径。
        
        .. image:: images/cpp_env3.png
            :scale: 80%

    Linker的Input菜单里的Additional Dependencies中添加daoai_dl_sdk.lib。

        .. image:: images/cpp_env4.png
            :scale: 80%

    Debugging的Environment菜单里的Path 添加 DWSDK_PATH\\bin, DWSDK_PATH\\3rdparty;

        .. image:: images/vs_3rdparty.png
            :scale: 80%


C# 项目配置
***************

    点击C#项目中的添加reference
        
        .. image:: images/add_ref.png
            :scale: 100%

    点击浏览，然后浏览解压目录下的bin文件夹内的 ``dl_sdk_net.dll`` 文件，勾选后，点击OK。
        
        .. image:: images/browse_dll.png
            :scale: 100%

    点击assembly，然后搜索 ``system.drawing`` 勾选后，点击OK。
        
        .. image:: images/browse_assembly.png
            :scale: 100%

Python Windows 环境配置
*********************************************

Python Windows SDK wheel 只支持Windows环境

需要首先安装 **Python 3.10** 

如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. code-block::

    python3 --version

请注意，安装包分为 **企业版** 和 **工业版** ，请下载您使用版本对应的SDK，以确保可以正常使用。

根据您的Python版本 从 `下载中心 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhfpGDv2VpZMi5NhkxrFvlwBthrgxdCccubfLp8LefGAQw?e=78ZGUn>`_ 下载 其中的2.24.7.0 版本的 .whl文件

或者从百度网盘里下载 https://pan.baidu.com/s/1gE2QuiVTaaMrVMAzVtRo2g?pwd=g4un 提取码: g4un 

使用以下命令安装wheel文件

.. code-block::

    pip install {wheel_file}.whl

然后您的DaoAI Python Windows SDK 模组就安装完毕了

您可以使用以下命令来导入模组。 

.. code-block:: python

    import dlsdk.dlsdk as dlsdk

您需要有有效的DaoAI 许可证才可以正常使用，如果您没有许可证，请参考 :ref:`软件许可证`


本地推理服务 （Inference Service）
******************************************

本地推理服务可以让您通过本地的HTTP请求来执行模型的推理并获取结果。

    .. image:: images/inf_service.png
        :scale: 100%

在安装目录下，您可以找到 inference_service.exe 以及 inference_service_gui.exe.

其中双击运行inference_service.exe 即可在后台启动推理服务，您可以在右下角任务图标中找到。而 inference_service_gui.exe 则会打开该服务的图形操作界面 （需要首先运行 inference_service.exe ）。

使用图形界面
~~~~~~~~~~~~~~~~

您可以通过图形界面注册和移除您的模型。

    .. image:: images/inf_gui.png
        :scale: 100%

点击 ``Add Model`` 来注册一个模型， 输入模型名称，选择模型的路径，然后选择模型的类型。点击 OK 即可注册模型。

删除模型时，选中一个模型，点击 ``Delete Model`` 即可删除模型。

使用HTTP请求
~~~~~~~~~~~~~~~~

您可以通过HTTP请求 发送至 localhost:5000 来管理您的模型，以及执行模型的推理并获取结果。

总共有4个请求可以使用，这里使用 Postman 来演示。

1. 注册模型：

    **Post** localhost:5000/register

    .. image:: images/inf_register.png
        :scale: 100%

    该命令可以讲本地的模型注册到推理服务里，可供之后的使用

2. 列出模型：

    **GET** localhost:5000/list

    .. image:: images/inf_list.png
        :scale: 100%

    该命令可以列出已经注册的模型

3. 模型推理：

    **Post** localhost:5000/inference

    .. image:: images/inf_inf.png
        :scale: 100%

    该命令指定一个模型，和一个本地图片路径，并获取该模型于指定图片的推理结果。

4. 模型删除：

    **DELETE** localhost:5000/delete

    .. image:: images/inf_del.png
        :scale: 100%

    该命令会讲指定的模型从推理服务中移除 (并不会删除本地模型文件)

使用C++ 
~~~~~~~~~~~~~~~~

C++ Inference Client 通过和 Inference Service 交互， 极简化了环境配置的依赖。

比起 Windows C++ SDK 需要使用许多的dll依赖， C++ Inference Client 只需要1个。避免了在项目想要引入其它依赖时（如 Opencv）的dll版本冲突。

C++ Inference Client 提供了与 Windows C++ SDK 相同的接口，详情请参考 :ref:`SDK接口文档`

您可以在安装目录下的 "C:\\Program Files\\DaoAI World SDK\\InferenceClient\\InferenceClientExample" 中找到 :ref:`C++ Inference Client 示例项目`


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

SDK接口文档
---------------

更详细的SDK，函数接口，数据结构等，请查阅SDK文档：

`C++ SDK 接口文档 <../_static/doc_C++/index.html>`_

`C# SDK 接口文档 <../_static/doc_Cs/index.html>`_

`C++ Inference Client 接口文档 <../_static/doc_C_client/index.html>`_

代码示例
-------------

.. toctree::
    :maxdepth: 1
    
    cpp_eg
    cs_eg
    python_win_eg
    cpp_client
    .. python_eg
