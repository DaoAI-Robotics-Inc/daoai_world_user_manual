Image Classification
==================================

**Image Classification** is used to classify different scenes. It's important to note that it is not designed to distinguish between different objects within the same scene.

    .. image:: Images/class.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_class-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.


Use Case Scenarios
----------------------------------

**Image Classification** can be used to distinguish between different scenes. It can also be used to differentiate between different objects. However, when using **Image Classification** for object differentiation, make sure that only the objects being classified are present in the scene.

**Image Classification** can also be applied to distinguish between different states of the same object. For example, whether an object is damaged or in different conditions. Again, ensure that only the object being classified is present in the scene.

Annotation Method
-------------------------

If you have a pre-trained model, you can use the assisted annotation tool, allowing the deep learning model to help with annotations. You can then verify and correct the annotations as needed.
    
    .. image:: Images/suppor_anno.png
        :scale: 100%

Choose a class and complete the annotation—each image can only have one class.

    .. image:: Images/classAnno0.png
        :scale: 100%

Notes
--------------

1. The annotation target for **Image Classification** is the entire image, not a single object within the image.

2. In a **Image Classification** dataset, each image should only contain the target object, reducing the impact of other objects.

3. Preprocessing techniques can be used to define the region of interest to minimize the influence of other objects in the image.

练习
--------

Download  `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ with image_classification.zip

After unzipping, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Later, you can upload both images and annotation files to compare the results.