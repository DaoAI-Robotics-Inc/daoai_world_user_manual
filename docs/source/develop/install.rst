DLSDK安装包
=============

安装
--------------

首先需要下载 `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_

    - **C++** 和 **C#** : DLSDK_C++_C#_master_66.zip

    把下载的zip文件解压后，会看到3个文件，分别是： `dlsdk_2.0_setup.exe`, `dlsdk_2.0_setup-1.bin` 和 `dlsdk_2.0_setup-1.bin` ，双击运行其中的 `dlsdk_2.0_setup.exe` 执行文件开始安装。

    .. image:: images/dlsk_installer_unzip.png
            :scale: 80%

    .. note:: 
        DLSDK 安装包需要磁盘中存在6.7GB以上的空间。

    - 选择DLSDK文件的安装目录，默认路径为： ``C:\Program Files\DLSDK`` 。

    .. image:: images/dlsk_installer_path.png
            :scale: 80%   
    
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
--------------

DLSDK 需要拥有 `DaoAI` 官方授权的软件许可证才能使用，请联系您的支持工程师或者客户获取许可证。您需要为 `DaoAI` 的工作人员提供您电脑的信息：

    - 双击桌面 ``DLSDK`` 快捷方式，打开许可证管理中心。

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

远程许可证
~~~~~~~~~~~~

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
        激活许可证需要电脑连接互联网。

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
------------

DLSDK 安装包会自动建立 DLSDK 所需的系统环境变量： ``DWSDK_PATH`` 。在使用 DLSDK 时可以直接引用此变量即可。

    .. image:: images/dlsk_installer_dwsdk_path.png
            :scale: 80%  