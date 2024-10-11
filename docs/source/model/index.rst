Instance Segmentation
==============================

In **Instance Segmentation** , the model locates objects within an image and generates precise boundary delineations. 
In real-world applications, **Instance Segmentation** is commonly used for recognizing objects with irregular shapes.

    .. image:: Images/insseg.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_insseg-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.


Use Case Scenarios
------------------------------

**Instance Segmentation** can be used to detect the number and location of one or more different objects in an image. The model is suitable for tasks that require simple object segmentation, classification, and localization.

In **Instance Segmentation**, you can either create a single object label for identifying one type of object, 
such as finding and separating a specific type of object among many mixed objects, 
or you can create and learn multiple object labels to achieve segmentation and recognition of 
various objects simultaneously.

Annotation Methods
------------------------

If you have a pre-trained model, you can use annotation tools to assist with labeling, then review and correct the annotations.

    .. image:: Images/suppor_anno.png
        :scale: 100%

Use the polygon tool or smart polygon tool to annotate the outer contours of the objects.

    .. image:: Images/segAnno4.png
        :scale: 100%

Repeat the annotation for all objects in the scene. If there are no objects in the scene, mark it as empty.

Notes
------------------

1. When naming labels, use descriptive tags. Descriptive tags significantly reduce the likelihood of annotation errors and facilitate the practical application of the model. Non-descriptive tags are more likely to cause annotation mistakes due to their weak correlation with the annotated objects, making it difficult to quickly assess the accuracy of the model's predictions.

2. If there is only one object to detect (i.e., only one label), you can annotate partially occluded objects (typically no more than 30%). However, avoid including objects where important geometric features are obscured by other objects.

3. If there are multiple objects to detect (including different sides of the same object), annotate objects that are not obscured by other objects or are only on top, and try to maintain the integrity of the objects.

4. When annotating polygons, annotate the actual boundaries in the image, not virtual boundaries. If part of the object is obscured by other objects, annotate the visible parts and avoid annotating the occluded parts.

.. image:: Images/example1.png
    :scale: 100%

Practice
------------

Download  `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ to obtain Instance_segmentation.zip

After extracting, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Afterward, you can upload both images and annotation files to compare the results.