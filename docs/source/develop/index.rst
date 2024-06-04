开发功能
=============

本章将详细介绍 DaoAI World 软件开发包 (SDK) 的配置和使用。DaoAI World SDK 提供了一套全面的工具，用于调用深度学习模型、处理输出及执行各种通用功能，以满足软件开发的需求。

在安装目录下有SDK的示例项目。

    .. image:: images/install_folder.png
        :scale: 100%

使用Visual Studio打开DLSDK Example.sln项目。

    .. image:: images/example_path.png
        :scale: 100%

    .. image:: images/vs.png
        :scale: 60%

项目分为C++项目，和C#项目，邮件点击properties, 然后选择启动项目，来选择运行C++或者C#项目。

    .. image:: images/start_up.png
        :scale: 70%

然后选择启动设置为release x64, 然后点击Local Windows Debugger 就可以运行项目了。

    .. image:: images/run_0.png
        :scale: 60%
        
    .. image:: images/run_1.png
        :scale: 60%

C++项目不需要额外进行环境配置。

使用DLSDK需要有效的使用许可证，详情请见 :ref:`DLSDK显示License Check Fail`

C# 环境配置
------------

首先需要将 DLSDK 安装目录下的bin目录和3rdparty目录添加到系统环境变量path下面。

如下图，安装目录为 C:\\Users\\daoai\\Downloads\\DLSDK, 那么就需要将一下两个目录添加到path系统变量中。

C:\\Users\\daoai\\Downloads\\DLSDK\\3rdparty， <DLSDK 目录>\\3rdparty
C:\\Users\\daoai\\Downloads\\DLSDK\\bin， <DLSDK 目录>\\bin

    .. image:: images/path_icon.png
        :scale: 70%

    .. image:: images/path_step.png
        :scale: 100%

如果使用 **C#** , 需要添加reference到C#项目。

第一步，点击添加reference
    .. image:: images/add_ref.png
        :scale: 100%

点击浏览，然后浏览安装目录下的bin文件夹内的 ``dl_sdk_net.dll`` 文件，勾选后，点击OK。
    .. image:: images/browse_dll.png
        :scale: 100%

点击assembly，然后搜索 ``system.drawing`` 勾选后，点击OK。
    .. image:: images/browse_assembly.png
        :scale: 100%



SDK
------

更详细的SDK，函数接口，数据结构等，请查阅SDK文档：

`C++ SDK 文档 <../_static/doc_C++/index.html>`_

`C# SDK 文档 <../_static/doc_Cs/index.html>`_


