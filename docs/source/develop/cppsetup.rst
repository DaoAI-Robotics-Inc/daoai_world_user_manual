DWSDK C++ Project Configuration
-------------------------------

Project Configuration
~~~~~~~~~~~~~~~~~~~~~~

The steps in the C++ sample project have already been configured. If you need to create a **custom project** or **start from an empty project**, the following configurations are required.

Right-click on the C++ project and open the properties.

.. image:: images/cpp_env1.png
    :scale: 70%

In the properties, select C++17.

.. image:: images/cpp17.png
    :scale: 80%

In the C++ settings, under the General menu, add the include folder path from the DW_SDK root directory to the **Additional Include Directories**.

.. image:: images/cpp_env2.png
    :scale: 80%

In the Linker settings, under the General menu, add the bin folder path from the DW_SDK root directory to **Additional Library Directories**.

.. image:: images/cpp_env3.png
    :scale: 80%

In the Linker **Input** menu, add `daoai_dl_sdk.lib` to **Additional Dependencies**.

.. image:: images/cpp_env4.png
    :scale: 80%

In the Debugging settings, under the Environment menu, add `DWSDK_PATH\\bin` and `DWSDK_PATH\\3rdparty` to the **Path**.

.. image:: images/vs_3rdparty.png
    :scale: 80%

Sample Project
~~~~~~~~~~~~~~

You can refer to the :ref:`C++ Code Example` for using the project.

SDK Interface Documentation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Detailed interface documentation can be found in the `C++ SDK Interface Documentation <../_static/doc_C++/index.html>`_.
