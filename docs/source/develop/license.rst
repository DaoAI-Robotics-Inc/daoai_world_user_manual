
软件许可证
------------------

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

    .. warning::
        
        license_manager.exe 在 2.24.7 版本中有更新，无法与2.24.6 版本的license_manager 兼容。尽管许可证文件是共用的，仍需要替换旧版本的license_manager.exe 才可以使用2.24.7版本的sdk. |br|

        license_manager 可以在 <sdk安装目录> 下的 bin文件夹中找到。


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
    
.. |br| raw:: html

      <br>

非监督模型授权
~~~~~~~~~~~~~~~~~~~~~~~~

非监督模型需要我们提供的 USB 加密狗，使用时需要插到部署的机器上即可。

插上加密狗后，运行sdk时，您会看到 Unsupervised Model SDK license: OK 

    .. image:: images/unsupervised_key.png
        :scale: 80%  

如果没有加密狗 或者没有正确授权的加密狗，则会看到 Unsupervised Model SDK license: Invalid

或者 弹窗报错。

    .. image:: images/ldk_failed.png
        :scale: 80%  
