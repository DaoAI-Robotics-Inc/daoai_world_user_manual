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

**Unsupervised Defect Segmentation** is suitable for the following scenarios:

1. **Single Object**: The dataset contains only one type of object, and its position must remain relatively fixed.
2. **Defect Detection**: The object can be in either a normal or abnormal (NG) state, enabling the detection of changes such as damage or deformation.

Model Detection Modes
------------------------------------------

The new **Unsupervised Defect Segmentation** model supports four detection modes, which can be selected based on specific requirements:

1. **Image-Level - Whole Image Detection**  
   - Analyzes the entire image to determine if defects are present.
   - Suitable for scenarios where precise localization is not required.

2. **Image-Level - Region Detection**  
   - Analyzes specific regions of the image to determine if defects exist within the target areas.
   - Suitable for scenarios with fixed target regions.

3. **Pixel-Level - Whole Image Detection**  
   - Analyzes each pixel in the image and highlights abnormal regions.
   - Suitable for scenarios requiring precise localization of defect areas.

4. **Pixel-Level - Region Detection**  
   - Analyzes each pixel within specific regions of the image and highlights abnormal areas.
   - Suitable for scenarios requiring precise annotation of target areas.

Annotation Methods
----------------

Annotation methods vary depending on the model detection mode, as described below:  

1. **Image-Level - Whole Image Detection**  
   Annotate each image as either **OK** (normal) or **NG** (abnormal).  

   .. image:: Images/img_whole_anno.png  
        :scale: 60%  

   Supports batch selection of images for unified annotation as **OK** or **NG**.  

   .. image:: Images/batch_annotate.png  
        :scale: 60%  

2. **Image-Level - Region Detection**  
   First, define a reference image and select the detection region by drawing a bounding box, then save it as a template. Since unsupervised detection requires objects to remain relatively fixed, the detection regions in all images must cover the same parts.  

   .. image:: Images/img_region_anno.png  
        :scale: 60%  

   Annotate by clicking on the detection regions and marking them as **OK** or **NG**.  

   .. image:: Images/img_region_anno.png  
        :scale: 60%  

3. **Pixel-Level - Whole Image Detection**  
   Use intelligent annotation tools or polygon tools to outline the defect regions.  

   .. image:: Images/pixel_whole_anno.png  
        :scale: 60%  

4. **Pixel-Level - Region Detection**  
   First, define a reference image and select the detection region by drawing a bounding box, then save it as a template.  

   .. image:: Images/pixel_region_anno.png  
        :scale: 60%  

   Use intelligent annotation tools or polygon tools to outline the defect regions within the detection area. If there are no defects, mark the image as defect-free.  

   .. image:: Images/pixel_region_anno2.png  
        :scale: 60%  

Notes
------------

1. **Data Consistency**  
   Normal images should not include any defect annotations, as this may lead to training failures or suboptimal results.

2. **Multiple Defects Support**  
   If an object has multiple defects, you can annotate each defect with a separate region.

3. **Data Ratio**  
   - **Normal Data Priority**: The number of normal images in the training set should be greater than or equal to the number of defect-free images.  

4. **Default Configuration**  
   The unsupervised defect segmentation model does not apply any data augmentation options by default during training.

5. **Built-in Image Splitting for Training**  
   During whole image detection, the training mode can be set to **Normal** or **High Precision**. High precision mode splits images into 512×512 or 256×256 resolutions for training, increasing detection time but significantly improving model accuracy.

.. note::  

    - The quality of defect-free data is crucial for unsupervised models; ensure that normal data is accurately annotated.  
    - Model performance depends on a reasonable ratio of normal and abnormal images.  

Practice
--------

Download the practice data `anomaly_detection.zip` from the following link:  
`Practice Data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_  

After extraction, you will obtain 11 images and their annotation files (.json).  
- Upload the images to DaoAI World for annotation practice.  
- After completing the annotations, upload the images and annotation files together to validate the model training results.
