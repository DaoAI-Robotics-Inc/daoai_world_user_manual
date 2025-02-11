安装
-------------------------

SDK版本
***********

SDK 和 DaoAI World 一样，分为 **工业版** 和 **企业版**。

两个版本的区别在于， **工业版** 可以支持工业版特有的两种类型的模型： :ref:`错漏装检测` 和 :ref:`定位模型` 。

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
    Windows C++ C# SDK 目录下的 2.24.8.0 安装包zip
    
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


系统环境变量
***************

DW_SDK 安装包会自动建立 DW_SDK 所需的系统环境变量： ``DWSDK_PATH`` 。在使用 DW_SDK 时可以直接引用此变量即可。

    .. image:: images/dlsk_installer_dwsdk_path.png
            :scale: 80%  

    **C++** 和 **C#** 的SDK安装包在安装后的安装目录下的 DWSDK 文件夹内包含有SDK以及SDK的示例项目。

        .. image:: images/install_folder.png
            :scale: 100%

    使用DW_SDK需要有效的使用许可证，详情请见 :ref:`软件许可证`

.. |br| raw:: html

      <br>