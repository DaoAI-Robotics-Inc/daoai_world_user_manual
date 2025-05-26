DWSDK Python Windows 环境配置
--------------------------------

项目配置
~~~~~~~~~~~~

Python Windows SDK wheel 只支持Windows环境

需要首先安装 **Python 3.10** 

如果您已经安装了Python, 您可以使用以下命令来确认您的版本

.. code-block::

    python3 --version

请注意，安装包分为 **企业版** 和 **工业版** ，请下载您使用版本对应的SDK，以确保可以正常使用。

根据您的Python版本 从 `下载中心 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhfpGDv2VpZMi5NhkxrFvlwBthrgxdCccubfLp8LefGAQw?e=78ZGUn>`_ 下载 其中的2.24.7.0 版本的 .whl文件

或者从百度网盘里下载 https://pan.baidu.com/s/1gE2QuiVTaaMrVMAzVtRo2g?pwd=g4un 提取码: g4un 

使用以下命令安装wheel文件

.. code-block::

    pip install {wheel_file}.whl

然后您的DaoAI Python Windows SDK 模组就安装完毕了

您可以使用以下命令来导入模组。 

.. code-block:: python

    import dwsdk.dwsdk as dwsdk

您需要有有效的DaoAI 许可证才可以正常使用，如果您没有许可证，请参考 :ref:`软件许可证`


示例项目
~~~~~~~~~~~~

您可以参考我们提供的 :ref:`Python Windows 代码示例`

