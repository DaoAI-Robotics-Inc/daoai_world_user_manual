Rotated Object Detection
==========================================

**Rotated Object Detection** is similar to object detection but can also detect the rotation state of the objects.

    .. image:: Images/rot_obj.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/daoaiworld_rotobj.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.

Use Case Scenarios
------------------------------------------

**Rotated Object Detection** is suitable for determining whether an object appears in a scene, the number of times the object appears, and the object's rotation state.

**Object Detection** can identify one or more objects in a scene and provide their general locations. While object detection supports multi-object recognition, it does not return exact positions. If precise location is needed, use **Instance Segmentation** or **Keypoint Detection** models.

Annotation Methods
----------------------

If you have a pre-trained model, you can use annotation tools to assist with labeling, then review and correct the annotations

    .. image:: Images/suppor_anno.png
        :scale: 80%

First, use the rectangle annotation tool to mark the bounding box, then move the mouse to rotate the box. 

    .. image:: Images/rot_objAnno0.png
        :scale: 80%

Repeat the annotation for all objects in the scene. If there are no objects in the scene, mark it as empty.

Notes
--------------

1. The **Rotated Object Detection** model only supports rectangular annotations. The annotated areas may slightly overlap but should not completely overlap.

2. Annotations must not extend beyond the image boundaries.

3. As with other annotation models, avoid annotating objects that are largely occluded. Only annotate the topmost or most visible object.

Practice
----------------

Download  `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ to obtain rotated_object.zip

After extracting, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Afterward, you can upload both images and annotation files to compare the results.