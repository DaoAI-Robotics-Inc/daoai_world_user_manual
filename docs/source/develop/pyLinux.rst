
.. Python Linux/Jetson Configuration
.. -----------------------------------

.. The Python Linux/Jetson API is only supported in Linux systems, and it is divided into two categories: Linux machines and Jetson machines.

.. If you want to use it in a Windows environment, please refer to :ref:`1. Using Docker Image` to configure a Docker virtual environment.

.. If you are using a Linux environment, you can skip step 1 and refer to :ref:`2. Installing DaoAI Python Linux/Jetson API Wheel` to use it directly.

.. 1. Using Docker Image
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. First, Docker needs to be installed.

.. Once installed, run Docker and open the command line. Enter and run the following command to download the Docker container. 
.. Please note that this step requires about 60GB of disk space.

.. To download the Docker image for a Linux machine, use the following command:

.. .. code-block::

..     docker pull daoairobotics/daoai_vision 

.. Or, if you are using a Jetson machine, you can use the following command to download the Docker image for Jetson machines:

.. .. code-block::

..     docker pull daoairobotics/daoai_vision:jetson

.. .. image:: images/pull_docker.png
..     :scale: 100%
        
.. After the download is complete, run the following commands to start the Docker image and enter the Linux environment of the virtual image:

.. .. code-block::

..     docker run -it --gpus=all -v <local_path>:/home/appuser/workdir daoairobotics/daoai_vision

.. Where:

.. 1. <local_path> is your local path, replace it with your actual path.
.. 2. /home/appuser/workdir is the virtual path within the Docker container, which will contain the files and folders from the path specified in step 1.
.. 3. daoairobotics/daoai_vision is the name of the Docker image.

.. After entering the command, you'll be logged into the Docker virtual image as appuser, as shown in the image below:

..     .. image:: images/pscmd.png
..         :scale: 100%

.. Next, please refer to :ref:`3. Activate Your Linux/Jetson SDK`

.. 2. Installing DaoAI Python Linux/Jetson API Wheel
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Depending on your machine, download the appropriate files from the `Download Center <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EhSzNT1zD61Pv8MbPMr_PM8BLRSiHwbywBsRHAEY5ILSiQ?e=DzrN2M>`_  

.. - If you're using a **Linux machine,** download the .whl file from the ``Linux_wheels`` directory for version 2.24.7.0, or

.. - If you're using a **Jetson device,** download the .whl file from the ``Jetson_wheels`` directory for version 2.24.7.0.

.. The Python Linux/Jetson SDK wheel is only supported on Linux environments. If you're using Windows, you'll need to use a Linux virtual machine or Docker Image.

.. The Python Linux/Jetson SDK supports the following models:

.. .. list-table::
..    :header-rows: 1

..    * - Model Type
..      - Fast Mode	
..      - Accurate Mode	
..      - Rotation Accurate Mode
..    * - Instance Segmentation
..      - √  
..      - √  
..      - x 
..    * - Keypoint Detection
..      - √  
..      - √  
..      - x  
..    * - Unsupervised Defect Segmentation	
..      - x
..      - x  
..      - 
..    * - Image Classification
..      - √   
..      - √  
..      - 
..    * - Object Detection
..      - √  
..      - √  
..      - 
..    * - Supervised Defect Segmentation
..      - √   
..      -  
..      - 
..    * - OCR
..      - x  
..      - 
..      - 

.. 2.a Linux Installation
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. Linux requires **Python 3.10** and **Nvidia Cuda Toolkit** 

.. To verify your Python version, use the following command:

.. .. code-block::

..     python3 --version


.. - **Default Installation Method** : The daoai_vision package provides a binary installation package (.whl file), making it easy for users to install in their environment. During installation, the .whl file will automatically install the necessary runtime dependencies, but it won’t automatically install deep learning frameworks (such as PyTorch, ONNX, OpenVINO), allowing users to choose their preferred version.

..     .. code-block::

..         pip install <Wheel-File>.whl

..     With this installation method, all supported packages will be installed, and you can run local inference directly. If you need specific versions of essential dependencies like PyTorch or ONNX, it’s recommended to install them manually after installation.

.. - **Deep Learning Library Installation** : If you want to install all the necessary deep learning libraries for local Python inference, use this command:

..     .. code-block::

..         pip install <Wheel-File>.whl[dl]

..     This command will install the required deep learning libraries for inference.


.. After this, your DaoAI Python Linux/Jetson SDK module will be installed.

.. Next, please refer to section :ref:`3. Activate Your Linux/Jetson SDK` 

.. 2.b Jetson Installation
.. ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. Jetson devices require **Python 3.8** and must use a venv virtual environment.

.. To verify your Python version, use this command:

.. .. code-block::

..     python3 --version

.. Then you need to create a venv virtual environment in your working directory, use the following commands:

.. .. code-block::

..     python3 -m venv <virtual_environment_name>
..     source <virtual_environment_name>/bin/activate


.. Then, install the wheel file, making sure the .whl file is present in the current terminal path.

.. - **Default Installation Method** ：daoai_vision provides a binary installation package (.whl file) for easy installation in your environment. During installation, the .whl file will automatically install the required runtime dependencies, but it will not install deep learning frameworks (such as PyTorch, ONNX, or OpenVINO), allowing users to select the versions they need.

..     .. code-block::

..         pip install <Wheel-File>.whl

..     With this installation method, all supported packages will be installed, and you can directly run local inference. If you need specific versions of important dependencies, such as PyTorch or ONNX, it is recommended to install them manually after installation.

.. - **Deep Learning Library Installation** : If you want to install all the necessary deep learning libraries for local Python inference, use this command:

..     .. code-block::

..         pip install <Wheel-File>.whl[dl]

..     This command will install the required deep learning libraries for inference.


.. Please note that you need to follow the official website to download the correct version of torchvision: https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048 

.. Alternatively, you can use the following commands to install torchvision version 0.16.1:

.. .. code-block::

..     sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev
..     git clone --branch v0.16.1 https://github.com/pytorch/vision torchvision

..     cd torchvision
..     export BUILD_VERSION=0.16.1
..     python3 setup.py install
..     cd ../ 


.. After this, your DaoAI Python Linux/Jetson SDK module will be installed.

.. Next, please refer to section :ref:`3. Activate Your Linux/Jetson SDK` 

.. 3. Activate Your Linux/Jetson SDK
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Next, you need to run the following command in the terminal to activate your license. If you do not have a license key and machine license file, please refer to :ref:`DW SDK License`

.. Once you acquired a License File, use the following command to activate your SDK

.. .. code-block::

..     daoai_vision activate --license ABCDEF-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX --machine <machine-file-path>

.. This command will validate your license file and cache it for 30 days; after that, you will need to run the command again.

.. .. note::

..     When re-running activation, you need to add the ``-r`` flag to clear the previous license cache. If you used an invalid license, you will also need to use the ``-r`` flag to retry activation.

..     .. code-block::
..         daoai_vision activate -r --license ABCDEF-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XX --machine <machine-file-path>


.. Once activation is successful, you can use the Python SDK normally. You can import the module using the following statement:

.. .. code-block:: python

..     import daoai_vision as dv

.. 4. Using the Python Linux/Jetson SDK
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. daoai_vision supports various inference deployment methods, including local deployment and remote hosting deployment. Depending on your needs, you can choose the appropriate deployment method to run model inference. There are mainly three deployment methods:

.. 1. Self-Hosted Deployment:
..     **Self-hosted deployment** runs model inference on your own hardware, with two main methods: **Native Python Deployment** and **Docker Server Deployment.**

..     **Native Python Deployment**

..     Run inference locally on your hardware using native Python. You can load the model and perform inference with the following code:

..     .. code-block:: python
        
..         import daoai_vision as dv
..         MODEL_ZIP = 'path/to/downloaded/zip'  # Model file (.zip / .dwm)
..         DEVICE = 'gpu'                        # 可选设备: cpu / gpu 
..         IMG_PATH = 'path/to/image'            # 待推理的图像路径

..         model = dv.get_model(model_path=MODEL_ZIP, device=DEVICE)
..         results = model.infer(IMG_PATH)

..     You can also run inference using the command line tool with the following command:

..     .. code-block::

..         daoai_vision infer -i /path/to/image.png -m /path/to/model.zip --native-python

..     This command will print a summary of the inference results in the terminal and save the results as a JSON file in the same directory as the input image.


..     **Docker Server Deployment**

..     Run an inference server using Docker, which can run in the background and accept inference requests.

..     1. Start the server:

..         .. code-block::

..             daoai_vision server enable

..     2. Run inference requests:
        
..         You can send inference requests via the command line tool:

..         .. code-block::

..             daoai_vision infer -i /path/to/image.png -m /path/to/model.zip

..         You can also set ``server=True`` in Python to send inference requests:

..         .. code-block:: python

..             import daoai_vision as dv
..             model = dv.get_model(model_path=MODEL_ZIP, server=True)
..             results = model.infer(IMG_PATH)

.. 2. Hosted Deployment
..     **Hosted deployment** allows sending inference requests to the remote inference server of DaoAI World. In this case, you need to specify the URL of the remote server.

..     Send requests via the command line:
    
..     .. code-block::

..         daoai_vision infer -i /path/to/image.jpg -m /path/to/model.zip -url http://remote-server.com:PORT


..     Use hosted inference in Python:

..     .. code-block:: python

..         import daoai_vision as dv
..         model = dv.get_model(model_path=MODEL_ZIP, server=True)
..         results = model.infer(IMG_PATH, url='http://remote-server.com:PORT')

