常见问题 
===========

.. contents::
    :local:

DLSDK显示 licensemanger_cli.exe is not rcognized as an internal or external command
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    如果在运行DL SDK项目时遇到以下的报错，这说明软件的licensemanager_cli.exe 和 machine.lic 没有在正确的位置    

    .. image:: images/licensemanager_notfound.png
        :align: center


    需要打开vs的项目设置，找到Debugging设置中的 Working Directory 路径，默认为项目文件的文件夹。然后需要将licensemanger_cli.exe 同license.lic文件复制到该位置。

    .. image:: images/working_dir.png
        :align: center

    .. image:: images/move.png
        :align: center

    如果使用的是Build后的exe, 则需要将licensemanger_cli.exe 同license.lic文件放置于exe的同目录内。

    重新运行项目即可解决。如果没有有效的license文件，请见下一条

DLSDK显示License Check Fail 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    如果在运行DL SDK时遇到以下的报错，这说明了软件在该电脑没有有效的license，无法使用软件。

    .. image:: images/failed.png
        :align: center


    这个时候你需要联系你的支持工程师、或者客服，获取权限。

    你的支持工程师和客服都会需要你电脑上的一个信息：机器码。该码是每台电脑上都不同的，并且唯一的，系统就是通过机器码来获取该电脑是否拥有使用权限。

    那么，如何查询你的机器码呢？

    首先，你需要输入以下代码至你的command prompt窗口（使用 **“WIN”** + **R**按键，输入 **cmd**，打开command prompt窗口）。

    .. code-block:: python
        
        python.exe -m pip install py-machineid

    |

    然后command prompt会自动运行并下载所需的文件。

    .. image:: images/cmd_install_machine_id.png
        :align: center


    然后，打开Python App，输入下方的指令，就可以查看到自己的机器码：

    .. code-block:: python
        
        import machineid
        print(machineid.id())

    |


    .. image:: images/checkmachineid.png
        :align: center
    
    请把上方的机器码提供给你的支持工程师或者客服，他们会帮助你获取权限。

    |


图片上传后，在标注页面打开是全黑的图片
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    .. image:: images/invalid_images.png
        :align: center

    上传图片时，DaoAI World 会对上传的数据进行检查和验证，部分图片可能会出现上传失败(如上图)。这是因为图片可能出现了损坏，建议重新采集图片。或者使用以下的方式尝试：

方法1：尝试修复损坏数据
-----------------------

    使用 **XnView MP** 图片工具， `下载连接 <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EWlgNZq_aBNFuomgwXGDx_QBzG2SBuqYFRd724qvd1TJXw?e=33ebHp>`_ 

    解压文件后，找到 **XnView MP** 图片工具的执行程序，双击运行。

    .. image:: images/xnviewmp_exe.png
        :align: center
        :scale: 75%

    左上角 `文件` ， `打开` 选取损坏的图片。

    .. image:: images/xnviewmp_open_file.png
        :align: center
        :scale: 75%
    
    左上角 `文件` ， `另存为` 保存图片到另外的路径下。

    .. image:: images/xnviewmp_save_as.png
        :align: center
        :scale: 75%

    将保存好的图片重新上传。

    .. image:: images/xnviewmp_upload_success.png
        :align: center
        :scale: 75%

    

    