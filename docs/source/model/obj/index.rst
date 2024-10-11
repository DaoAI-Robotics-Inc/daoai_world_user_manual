Object Detection
===============================

**Object detection** is used to identify the types and quantities of objects in a scene. Unlike instance segmentation models, object detection does not precisely identify the object's contours or exact location.

    .. image:: Images/obj.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_obj-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.


Use Case Scenarios
----------------------------------

**Object Detection** is suitable for determining whether an object is present in a scene or for counting how many times a certain object appears.

**Object Detection** can be used to recognize the number of occurrences and approximate locations of one or more objects in a scene. 
It can detect multiple object types simultaneously, but it does not provide the exact location of the objects. For precise localization, you can use an **Instance Segmentation** model or a **Keypoint Detection** model.

Annotation Method
-------------------

If you have a pre-trained model, you can use the assisted annotation tool to let the deep learning model help you annotate, then inspect and correct the annotations if necessary.

    .. image:: Images/suppor_anno.png
        :scale: 100%

Use the rectangular annotation tool or smart annotation to create bounding boxes around the objects.

    .. image:: Images/objAnno1.png
        :scale: 100%

Repeat the annotation process for all objects in the scene. If there are no objects in the scene, annotate it as empty.

Notes
--------------

1. The **Object Detection** model only supports rectangular annotations. Therefore, in object detection annotations, the annotated regions may slightly overlap but should not fully overlap.

2. The annotation region for the **Object Detection** model must not exceed the image boundaries.

3. As with other annotation models, avoid annotating objects that are heavily obscured. Instead, focus on annotating the topmost or most visible objects.


Practice
----------

Download the `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ with object_detection.zip.

After unzipping, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Later, you can upload both the images and annotation files to compare the results.