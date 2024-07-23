开发功能
=============

    本章将详细介绍 DaoAI World 软件开发包 (SDK) 的配置和使用。DaoAI World SDK 提供了一套全面的工具，用于调用深度学习模型、处理输出及执行各种通用功能，以满足软件开发的需求。

    首先需要下载 `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_

    **C++** 和 **C#** : DaoAI_World_C++_C#_SDK_2.24.5.0_28.zip
    **Python Windows** : DaoAI_World_Python_SDK_Windows_2.24.5.0 / dlsdk-1.0.1-cp310-cp310-win_amd64.whl
    **Python Linux/Jetson** : DaoAI_World_Python_SDK_Linux_Jetson_2.24.5.0/

    **C++** 和 **C#** 的SDK解压后在解压目录下包含有SDK以及SDK的示例项目。

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

    使用DLSDK需要将有效的许可证管理器移动至项目路径，详情请见 :ref:`DLSDK显示 licensemanger_cli.exe is not rcognized as an internal or external command`

    使用DLSDK需要有效的使用许可证，详情请见 :ref:`DLSDK显示License Check Fail`

硬件需求
-----------

DaoAI World SDK 支持CPU模式和GPU模式。尽管您没有GPU 也可以使用DaoAI World 深度学习模型使用CPU进行预测。

当使用GPU模式时，模型的运行时间会显著快于CPU模式。

使用GPU模式的最低需求为:

- 显卡: **Nvidia 1050Ti 显卡, 4GB 显存**

- 显卡驱动： **GeForce Game Ready Driver 驱动版本：552.22， 发布于 2024年 4月16日**


C++ 环境配置
------------

    
    首先需要将 DLSDK 解压目录下的bin目录和3rdparty目录添加到系统环境变量path下面。

    如下图，解压目录为 C:\\Users\\daoai\\Downloads\\DLSDK, 那么就需要将一下两个目录添加到path系统变量中。

    C:\\Users\\daoai\\Downloads\\DLSDK\\3rdparty， <DLSDK 目录>\\3rdparty
    C:\\Users\\daoai\\Downloads\\DLSDK\\bin， <DLSDK 目录>\\bin

        .. image:: images/path_icon.png
            :scale: 70%

        .. image:: images/path_step.png
            :scale: 100%


    C++的示例项目中以下的步骤已经配置好了，如果您需要创建一个自定义项目，则需要进行以下的配置。

    打开项目后，右键点击c++的项目，然后打开属性。
        
        .. image:: images/cpp_env1.png
            :scale: 70%

    打开C++, 在General菜单里的Additional Include Directories中添加 DLSDK 根目录下的 include 文件夹路径。

        .. image:: images/cpp_env2.png
            :scale: 80%

    打开Linker, 在General菜单里的Additional Library Directories中添加 DLSDK 根目录下的 bin 文件夹路径。
        
        .. image:: images/cpp_env3.png
            :scale: 80%

    Linker的Input菜单里的Additional Dependencies中添加daoai_dl_sdk.lib。

        .. image:: images/cpp_env4.png
            :scale: 80%

C# 环境配置
------------

    首先需要将 DLSDK 解压目录下的bin目录和3rdparty目录添加到系统环境变量path下面。

    如下图，解压目录为 C:\\Users\\daoai\\Downloads\\DLSDK, 那么就需要将一下两个目录添加到path系统变量中。

    C:\\Users\\daoai\\Downloads\\DLSDK\\3rdparty， <DLSDK 目录>\\3rdparty
    C:\\Users\\daoai\\Downloads\\DLSDK\\bin， <DLSDK 目录>\\bin

        .. image:: images/path_icon.png
            :scale: 70%

        .. image:: images/path_step.png
            :scale: 100%


    第一步，点击C#项目中的添加reference
        
        .. image:: images/add_ref.png
            :scale: 100%

    点击浏览，然后浏览解压目录下的bin文件夹内的 ``dl_sdk_net.dll`` 文件，勾选后，点击OK。
        
        .. image:: images/browse_dll.png
            :scale: 100%

    点击assembly，然后搜索 ``system.drawing`` 勾选后，点击OK。
        
        .. image:: images/browse_assembly.png
            :scale: 100%

Python Windows 环境配置
---------------------------------

Python Windows SDK wheel 只支持Windows环境

需要首先安装 **Python 3.10** 

如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. code-block::

    python3 --version

根据您的Python版本 从 `下载中心 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=wVmvlv>`_ 下载  **Python Windows** : DaoAI_World_Python_SDK_Windows_2.24.5.0 目录下的.whl文件

使用以下命令安装wheel文件

.. code-block::

    pip install dlsdk-1.0.1-cp310-cp310-win_amd64.whl

然后您的DaoAI Python Windows SDK 模组就安装完毕了

您可以使用以下命令来导入模组。 

.. code-block:: python

    import dlsdk.dlsdk as dlsdk

您需要有有效的DaoAI 许可证才可以正常使用，如果您没有许可证，请参考:ref:`DLSDK显示License Check Fail`

Python Linux/Jetson 环境配置
---------------------------------

Python Linux/Jetson API 只支持在linux系统中使用，如果您想在Windows环境中使用， 那么请参考 :ref:`1. 使用Docker Image` 配置Docker 虚拟环境。

如果您使用的是linux环境，可以跳过 1. 并参考 :ref:`2. 安装DaoAI Python Linux/Jetson API Wheel` 来直接使用

1. 使用Docker Image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

需要先安装Docker。

安装完成后, 运行Docker 然后打开命令栏, 输入并运行以下命令 来下载Docker Container。注意 这一步需要预留约60G的磁盘空间。

.. code-block::

    docker pull daoairobotics/daoai_vision 

.. image:: images/pull_docker.png
    :scale: 100%
        
下载完毕后，运行以下命令运行Docker Image 并进入虚拟映像的linux环境

.. code-block::

    docker run -it --gpus=all -v <本地路径>:/home/appuser/workdir -e ACTIVATION_PATH_LICENSE='/home/appuser/workdir/license.lic' -e ACTIVATION_PATH_MACHINE='/home/appuser/workdir/machine.lic' -e ACTIVATION_FINGERPRINT='857f67d5-f6a2-4bb0-9af4-90dc72d58e73' daoairobotics/daoai_vision

其中

1. <本地路径> 是您本地的路径，请替换为您实际的路径。
2. /home/appuser/workdir 是Docker容器内的虚拟路径，该路径会包含第一步中指定路径的文件和文件夹。
3. -e ACTIVATION_PATH_LICENSE='/home/appuser/workdir/license.lic' 是DaoAI的许可证文件路径，该文件应放置在第一步设置的本地路径中，并且可以在虚拟路径中访问。如果您没有许可证，请参考:ref:`DLSDK显示License Check Fail`
4. -e ACTIVATION_PATH_MACHINE='/home/appuser/workdir/machine.lic' 是DaoAI的机器码文件路径，该文件应放置在第一步设置的本地路径中，并且可以在虚拟路径中访问。如果您没有许可证，请参考:ref:`DLSDK显示License Check Fail`
5. -e ACTIVATION_FINGERPRINT='857f67d5-f6a2-4bb0-9af4-90dc72d58e73' 是您机器的机器码。如果您不知道您的机器码是什么，请参考 :ref:`DLSDK显示License Check Fail`
6. daoairobotics/daoai_vision 是Docker镜像的名称。

输入命令后，如下图，您将以appuser登入Docker虚拟映像

    .. image:: images/pscmd.png
        :scale: 100%


2. 安装DaoAI Python Linux/Jetson SDK Wheel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Python Linux/Jetson SDK wheel 只支持linux环境，如果您使用的是Windows系统，那么请使用linux虚拟机，或者Docker Image

需要首先安装 **Python 3.10** 并且安装 **Nvidia Cuda Toolkit**

如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. code-block::

    python3 --version


根据您的Python版本 从 `下载中心 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=wVmvlv>`_ 下载 DaoAI_World_Python_SDK_Linux_Jetson_2.24.5.0/ 下的 .whl 文件

使用以下命令安装wheel文件

.. code-block::

    pip install daoai_vision-0.0.1-py3-none-any.whl 
    pip install dezip-0.0.0-cp310-cp310-linux_x86_64.whl 


然后您的DaoAI Python Linux/Jetson SDK 模组就安装完毕了

接下来您需要在终端中运行以下命令来激活您的许可证，如果您没有许可证，请参考:ref:`DLSDK显示License Check Fail`

.. code-block:: 

    daoai_vision activate --machine /path/to/machinefile  --license /path/to/licensefile

该命令会验证您的许可证文件，并缓存30天，之后需要重新运行命令。

您可以使用import daoai_vision 来导入模组。 

.. code-block:: python

    import daoai_vision as dv


.. note::
    如果您使用的是无界面应用，或者看到以下报错：

    .. code-block::

        ImportError: libGL.so.1: cannot open shared object file: No such file or directory

    请运行以下命令 安装 opencv-python-headless
    
    .. code-block::

        pip install opencv-python-headless
 

SDK
------

更详细的SDK，函数接口，数据结构等，请查阅SDK文档：

`C++ SDK 文档 <../_static/doc_C++/index.html>`_

`C# SDK 文档 <../_static/doc_Cs/index.html>`_

代码示例
-------------

代码示例有C++和C#两个示例项目：

.. toctree::
    :maxdepth: 1
    
    cpp_eg
    cs_eg
    python_eg

桌面应用程序
-------------

桌面应用程序使用C++，基于DaoAI World SDK开发，主要功能是实现深度学习的推理，并输出推理结果及可视化，供客户参考。

.. toctree::
    :maxdepth: 1

    cpp_demo