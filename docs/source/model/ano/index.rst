Anomaly Detection
==========================================

**Anomaly Detection** can identify whether an object is in an abnormal state, such as damage or deformation.

    .. image:: Images/ano.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_ano-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.

Use Case Scenarios
------------------------------------------

**Anomaly Detection** works on a single object. That is, the dataset should contain only one type of object, and the object's position must remain relatively fixed. The object will be classified as either normal or abnormal.

**Anomaly Detection** will learn to identify whether an object is in an abnormal state, using polygon annotations to mark the abnormal regions.

Annotation Method
------------------------

If a pre-trained model exists, you can use the assisted annotation tool, allowing the deep learning model to help with annotations. You can then verify and correct the annotations as needed.
    
    .. image:: Images/suppor_anno.png
        :scale: 100%

If the object has no defects, label it as good.

    .. image:: Images/anoAnno2.png
        :scale: 100%

If the object has defects, mark it as abnormal. Use the polygon tool or smart polygon to annotate the outline of the defect area.

    .. image:: Images/anoAnno1.png
        :scale: 100%

Notes
------------

1. In an **Anomaly Detection** project, there should only be **one label** used to annotate the abnormal areas on an object.

2. Objects annotated as **good** should not include any defect region labels. Otherwise, it may lead to unsatisfactory training results or even training failure.

.. note::

    1. During the training process of an Anomaly Detection project, ensure that the number of images assigned to the training set is less than or equal to the total number of defect-free images in the dataset. Otherwise, training may fail. Also, having too few defect images in the dataset may result in poor training outcomes.
    
    2. Unlike other projects, Anomaly Detection does not apply any data augmentation options by default.

Practice
----------

Download  `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ with anomaly_detection.zip.

After unzipping, you will get 11 images and annotation (.json) files. Upload only the images to DaoAI World for annotation practice. Later, you can upload both images and annotation files to compare the results.