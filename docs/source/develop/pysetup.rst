DWSDK Python Windows Environment Configuration
----------------------------------------------

Project Configuration
~~~~~~~~~~~~~~~~~~~~~

The Python Windows SDK wheel only supports Windows environments.

You need to install **Python 3.10** first.

If you have already installed Python, you can check your version with the following command:

.. code-block::

    python3 --version

Please note that the installation package is divided into **Enterprise Edition** and **Industrial Edition**. Please download the SDK version corresponding to the one you are using to ensure proper functionality.

Based on your Python version, download the .whl file for version 2.24.7.0 from the `Download Center <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhfpGDv2VpZMi5NhkxrFvlwBthrgxdCccubfLp8LefGAQw?e=78ZGUn>`_.

Alternatively, you can download it from Baidu Netdisk at https://pan.baidu.com/s/1gE2QuiVTaaMrVMAzVtRo2g?pwd=g4un with the extraction code: g4un.

Use the following command to install the wheel file:

.. code-block::

    pip install {wheel_file}.whl

Your DaoAI Python Windows SDK module will be installed.

You can import the module using the following command:

.. code-block:: python

    import dlsdk.dlsdk as dlsdk

You need a valid DaoAI license to use the SDK properly. If you do not have a license, please refer to :ref:`Software License`.

Example Project
~~~~~~~~~~~~~~~~~

You can refer to the :ref:`Python Windows code example` we provide.
