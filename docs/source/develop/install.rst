DW_SDK Windows Installation Package
-----------------------------------

SDK Version
***********

Like DaoAI World, the SDK is available in two versions: **Industrial Edition** and **Enterprise Edition**.

The difference between the two versions is that the **Industrial Edition** supports two types of models unique to industrial applications: :ref:`Defect Detection` and :ref:`Localization Models`.

Other than that, both versions are identical in terms of model usage and performance.

Hardware Requirements
*********************

DaoAI World SDK supports both CPU and GPU modes. Even if you do not have a GPU, you can still use the DaoAI World deep learning models for predictions using the CPU.

When using GPU mode, the runtime of the models will be significantly faster than in CPU mode.

The minimum GPU requirements for using GPU mode are:

- Graphics Card: **Nvidia 1050Ti, 4GB VRAM**
- GPU Driver: **GeForce Game Ready Driver, version 552.22, released on April 16, 2024**

Installation
************

    First, download the `DaoAI World SDK <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhJ2c8mQ3yZKuXUno9Vg1ucBCuvQzJZCyAhXnjbQnf7UNg?e=U1N81x>`_
    Windows C++ C# SDK package (version 2.22.7.0) ZIP file.

    Please note that the installation package is available for both **Enterprise Edition** and **Industrial Edition**. Be sure to download the SDK version that matches your usage to ensure it works properly.

    Alternatively, you can download it from Baidu Cloud at https://pan.baidu.com/s/1gE2QuiVTaaMrVMAzVtRo2g?pwd=g4un (extraction code: g4un).

    After extracting the ZIP file, you will see three files: `daoai_world_sdk_2.0_setup.exe`, `daoai_world_sdk_2.0_setup-1.bin`, and `daoai_world_sdk_2.0_setup-1.bin`. Double-click the `daoai_world_sdk_2.0_setup.exe` file to start the installation.

    .. image:: images/dlsk_installer_unzip.png
        :scale: 80%

    .. note::

        The DW_SDK installation package requires at least 6.7GB of free disk space.

    - Open the installer, and the default installation path will be: ``C:\Program Files\DaoAI World SDK``.

    - Choose ``Create Desktop Shortcut`` to easily manage the SDK software license.

    .. image:: images/dlsk_installer_desktop_shortcut.png
        :scale: 80%

    - Click ``Install`` to begin the installation process, and please be patient while it installs.

    .. image:: images/dlsk_installer_install.png
        :scale: 80%

    - Once the installation is complete, click ``Finish``.

    .. image:: images/dlsk_installer_done.png
        :scale: 80%

System Environment Variables
***************************

The DW_SDK installation package will automatically create the system environment variable ``DWSDK_PATH`` required for DW_SDK. You can directly reference this variable when using DW_SDK.

    .. image:: images/dlsk_installer_dwsdk_path.png
        :scale: 80%

The **C++** and **C#** SDK installation packages will contain both the SDK and SDK example projects in the DWSDK folder within the installation directory.

    .. image:: images/install_folder.png
        :scale: 100%

Using DW_SDK requires a valid software license. For more details, see :ref:`Software License`.

.. |br| raw:: html

      <br>
