Unsupervised Defect Segmentation
==========================================

**Unsupervised Defect Segmentation** is a model trained on normal data that can identify whether an object is in an abnormal (NG) state, such as damage, deformation, etc.  

    .. image:: Images/ano.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_ano-v8.mp4" type="video/mp4">
            </video>
        </div>

|

After completing model training, refer to the video in the :ref:`Training` section to create a dataset version and deploy the model.

Use Cases
---------------------

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_unsupervised_v8.mp4" type="video/mp4">
            </video>
        </div>

|

**Unsupervised Defect Segmentation** is suitable for the following scenarios:

1. **Single Object**: The dataset contains only one type of object, and its position must remain relatively fixed.
2. **Defect Detection**: The object can be in either a normal or abnormal (NG) state, enabling the detection of changes such as damage or deformation.


Model Detection Modes
------------------------------------------

**Unsupervised Defect Segmentation** supports two detection modes. Choose the appropriate one based on your use case:

1. **Image-Level**
   - Analyzes specific regions within the image to determine whether defects exist in those target areas.

2. **Pixel-Level**
   - Analyzes each pixel within specific regions of the image to locate and segment abnormal areas.

Annotation Methods
---------------------

In unsupervised defect segmentation projects, each detection mode requires a different annotation approach, as described below:

1. **Image-Level**
   First, define a reference image and draw detection regions on it. Save the result as a template.
   Since unsupervised detection assumes fixed object positions, all images should use the same layout of detection regions to cover the target parts.

    .. image:: Images/img_region_anno.png
        :scale: 60%

   During annotation, click on each detection region and label it as either **OK** or **NG**.

    .. image:: Images/img_region_anno2.png
        :scale: 60%

2. **Pixel-Level**
   Again, start by defining a reference image and selecting the detection regions, then save the result as a template.

    .. image:: Images/pixel_region_anno.png
        :scale: 60%

   Use the intelligent annotation tool or polygon tool to outline the defects within the detection region.
   If the image has no defects, label it as defect-free.

    .. image:: Images/pixel_region_anno2.png
        :scale: 60%

Notes
------------

1. **Training with Normal Samples Only**
   Only normal samples are used during training. In fact, only one reference image is used to train the model. The rest of the normal samples help the model determine a reasonable threshold for what is considered "normal."

.. note::

    During prediction, the model generates an AI Deviation Score ranging from 0 to 1.
    - A score of `0` means the sample is identical to the reference image.
    - A score close to `0` indicates high similarity to the normal sample.
    - A score of `1` means the sample is completely abnormal.
    - The model automatically sets a threshold based on the training set.
    Samples below the threshold are considered **normal**, while those above are considered **abnormal**.

2. **Data Consistency**
   Detection regions in normal images must not contain any defects. Otherwise, the training may fail or yield poor results.

3. **Multiple Defects Support**
   If an object has multiple defects, you may annotate each defect area separately.

4. **No Default Augmentation**
   No data augmentation is applied by default during training with the unsupervised defect segmentation model.

5. **Training Modes**
   There are two training modes: **Fast** and **Accurate**.
   - Fast mode compresses each detection region to 256×256.
   - Accurate mode compresses each region to 512×512 for training.
   If your image resolution is very high, it is recommended to split it into smaller detection regions to reduce resolution per region.

Practice
--------

Download the practice data `unsupervised_data.zip` from the following link:  
`Practice Data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_  

After extraction, you will obtain 11 images and their annotation files (.json).  
- Upload the images to DaoAI World for annotation practice.  
- After completing the annotations, upload the images and annotation files together to validate the model training results.
