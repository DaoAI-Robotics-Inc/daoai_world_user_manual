常见问题 
===========

.. contents::
    :local:



许可证错误, 调用 Initialize() 报错
-------------------------------------

    如果在运行DW SDK项目时遇到以下的报错，这说明软件的licensemanager_cli.exe 没有在正确的位置

    .. image:: images/licensemanager_notfound.png
        :align: center

    首先检查一下重启电脑是否能够解决。

    检查步骤 1
        如果您之前安装过旧版本的 DWSDK 或者 DaoAI Vision Pilot 以及 DaoAI Inspectra. 那么这些软件可能同时安装了旧版本的License Manager，并且由于环境配置关系，系统使用了这些旧版本的License Manager。

    解决方案 1
        在系统盘中搜索 licence_manager 并删除其它的licensemanager_cli.exe licensemanager_gui.exe 只保留 DWSDK 安装目录下的bin 文件夹里的licensemanager_cli.exe 和 licensemanager_gui.exe 
    
    检查步骤 2
        检查在 DWSDK安装目录下 （C:\\Program Files\\DaoAI World SDK\\DWSDK\\bin） 中是否存在 licensemanager_cli.exe 和 licensemanager_gui.exe。
        检查系统环境变量中 是否有路径正确指向了 C:\\Program Files\\DaoAI World SDK\\DWSDK\\bin 并且置顶， 如果是 %DWSDK_PATH%/bin  需要检查 %DWSDK_PATH% 是否正确指向C:\\Program Files\\DaoAI World SDK\\DWSDK

    解决方案 2
        修改系统环境变量中的错误指向，确保path中包含（C:\\Program Files\\DaoAI World SDK\\DWSDK\\bin）（C:\\Program Files\\DaoAI World SDK\\DWSDK\\3rdparty） 并且置顶。
    
    检查步骤 3
        删除 Windows %temp% 目录下的 DaoAI 文件夹，然后重新运行SDK程序。

    .. image:: images/temp.png
        :align: center

    解决方案 3 
        您使用的可能是一个损坏的模型，请重新训练并导出模型，避免使用已损坏的模型

Python Windows Example 运行时报错 ImportError: No module named cv2
------------------------------------------------------------------------

如果您尝试运行 Python Windows 时遇到以下报错

    .. code-block:: console

        ImportError: No module named cv2

请运行以下命令 安装 opencv-python
    
    .. code-block:: console

        pip install opencv-python

Python Windows Example 运行时报错 ImportError: DLL load failed while importing dlsdk: the specifid module could not be found
----------------------------------------------------------------------------------------------------------------------------------------------

如果您尝试运行 Python Windows 时遇到以下报错

    .. code-block:: console

        ImportError: DLL load failed while importing dlsdk: the specifid module could not be found.

这是由于Windows Store里的Python的问题。请删除Windows Store里的Python 并从 `官方途径 <https://www.python.org/downloads/>`_ 安装。


Python Linux/Jetson SDK 运行时报错 ImportError: libGL.so.1: cannot open shared object file: No such file or directory
---------------------------------------------------------------------------------------------------------------------------------------

    如果您使用的是无界面应用，可能会看到以下报错：

    .. code-block:: console

        ImportError: libGL.so.1: cannot open shared object file: No such file or directory

    请运行以下命令 安装 opencv-python-headless
    
    .. code-block:: console

        pip install opencv-python-headless


Python Linux/Jetson SDK 运行时报错 ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory
---------------------------------------------------------------------------------------------------------------------------------------

    如果您看到以下报错

    .. code-block:: console

        ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory

    请运行以下命令 安装 libglib2.0-0
    
    .. code-block:: console

        sudo apt update
        sudo apt install libglib2.0-0
