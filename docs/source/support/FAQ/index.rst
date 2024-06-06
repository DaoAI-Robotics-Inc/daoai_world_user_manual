常见问题 
===========

.. contents::
    :local:

DLSDK显示 licensemanger_cli.exe is not rcognized as an internal or external command
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    如果在运行DL SDK项目时遇到以下的报错，这说明软件的licensemanager_cli.exe 没有在正确的位置    

    .. image:: images/licensemanager_notfound.png
        :align: center


    需要打开vs的项目设置，找到Debugging设置中的 Working Directory 路径，默认为项目文件的文件夹。然后需要将licensemanger_cli.exe 同license文件复制到该位置。

    .. image:: images/working_dir.png
        :align: center

    .. image:: images/move.png
        :align: center

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