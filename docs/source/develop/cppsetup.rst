DWSDK C++ 项目配置
--------------------

项目配置
~~~~~~~~~~~~

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

示例项目
~~~~~~~~~~~~

项目的使用可以参考我们为您准备的 :ref:`C++ 代码示例`


SDK 接口文档
~~~~~~~~~~~~~~

详细的接口文档可以查阅 `C++ SDK 接口文档 <../_static/doc_C++/index.html>`_