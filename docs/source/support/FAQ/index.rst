常见问题 
===========

.. contents::
    :local:

DLSDK显示License Check Fail
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    如果在尝试使用软件时，弹窗提醒 **License Check Failed. Contact Administrator** 的提醒，这说明了软件在该电脑没有有效的license，无法使用软件。

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