Frequently Asked Questions
============================

.. contents::
    :local:

Python Linux/Jetson SDK Runtime Error ImportError: libGL.so.1: cannot open shared object file: No such file or directory
---------------------------------------------------------------------------------------------------------------------------------------

    If you're using headless application, you might encounter the following error:

    .. code-block::

        ImportError: libGL.so.1: cannot open shared object file: No such file or directory

    To resolve this, run the following command to install ``opencv-python-headless``
    
    .. code-block::

        pip install opencv-python-headless


Python Linux/Jetson SDK Runtime Error ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory
---------------------------------------------------------------------------------------------------------------------------------------

    If you see the following error:

    .. code-block::

        ImportError: libgthread-2.0.so.0: cannot open shared object file: No such file or directory

    Run the following commands to install ``libglib2.0-0``
    
    .. code-block::

        sudo apt update
        sudo apt install libglib2.0-0



DW_SDK Error licensemanger_cli.exe is not rcognized as an internal or external command
----------------------------------------------------------------------------------------------------------------------------

    If you encounter the following error when running a DL SDK project, it means that ``licensemanager_cli.exe`` and ``machine.lic`` are not in the correct location:

    .. image:: images/licensemanager_notfound.png
        :align: center

    You need to open the project's settings in Visual Studio, navigate to the Debugging settings, and check the ``Working Directory`` path, which defaults to the project's folder. Copy ``licensemanger_cli.exe`` and ``license.lic`` file to this directory.

    .. image:: images/working_dir.png
        :align: center

    .. image:: images/move.png
        :align: center

    If you're using the built exe, place both ``licensemanger_cli.exe`` and ``license.lic`` in the same directory as the executable.

    Re-running the project should resolve the issue. If you don't have a valid license file, please see the next section.


Uploaded images appear completely black in the annotation page
---------------------------------------------------------------

    .. image:: images/invalid_images.png
        :align: center

    When uploading images, DaoAI World checks and validates the data. Some images may fail to upload (as shown above). This might be due to corrupted images. You are advised to re-capture the images or try the following:

Attempt to repair the corrupted data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    Use the **XnView MP**  image tool, which you can download from the following link: `XnView MP <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EWlgNZq_aBNFuomgwXGDx_QBzG2SBuqYFRd724qvd1TJXw?e=33ebHp>`_ 

    Extract the files, find the **XnView MP**  executable, and double-click to run it.

    .. image:: images/xnviewmp_exe.png
        :align: center
        :scale: 50%

    Go to ``File`` -> ``Open`` to select the corrupted image.

    .. image:: images/xnviewmp_open_file.png
        :align: center
        :scale: 50%
    
    Go to ``File`` -> ``Save As`` and save the image to a new location.

    .. image:: images/xnviewmp_save_as.png
        :align: center
        :scale: 50%

    Upload the saved image again.

    .. image:: images/xnviewmp_upload_success.png
        :align: center
        :scale: 50%

    
    
Model inference fails when the object is at the edge
----------------------------------------------------
    
    If you find that the model fails to match the object and produces an error when the object is at the edge:

    .. image:: images/roi_fail.png
        :align: center
        :scale: 50%


    Check whether the ROI (Region of Interest) setting was applied during model training and if the object appears outside the ROI.

    .. image:: images/roi_fail_setting.png
        :align: center
        :scale: 70%

    
    Since the ROI preprocessing trims background areas and keeps only the area of interest, when an object is outside the ROI, it is considered part of the background and gets trimmed, resulting in no prediction.

    To resolve this issue, consider:

    1. Updating the ROI area by redefining the ROI to ensure it covers the area where the object may appear. Then, retrain the model.
    2. Removing the ROI preprocessing and retraining the model.

    This will allow you to correctly predict objects at the image's edge.

    .. image:: images/roi_fail_result.png
        :align: center
        :scale: 50%

Model inference fails due to different image resolutions
------------------------------------------------------------
    
    If the model fails during inference, check whether the resolution used during inference matches the resolution of the training data.

    In general, models can tolerate some changes in resolution. However, if an ROI preprocessing step is involved, the ROI will be based on the original image resolution.

    If you use a much lower resolution image for inference than the one used during training, the entire image may be cropped by the ROI, leading to inference failure.

    For example, if the training images have a width of 8000 pixels and the ROI preprocessing crops 1000 pixels off each side, using a 1000-pixel wide image for inference will result in the ROI cropping out the entire image, causing inference to fail.

    .. image:: images/res_fail.JPG
        :align: center
        :scale: 50%


    Ensure you use images with the same resolution as the training set when testing and inferring.
