Supervised Defect Segmentation
==========================================

**Supervised Defect Segmentation** can identify whether an object is in an abnormal state(NG), such as damage or deformation. Unlike Unsupervised Defect Segmentation, Supervised Defect Segmentation supports detecting multiple types of anomalies.

    .. image:: Images/sem.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_semseg-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.


Use Case Scenarios
------------------------------------------

**Supervised Defect Segmentation** focuses on a single object, where the dataset contains only one type of object, and its position needs to remain relatively fixed. This object is classified into normal and abnormal states.

The primary use cases for **Supervised Defect Segmentation** are:

    1. **Segment Specific Areas** : Identify changes in specific areas. For example, detecting fire lanes; if there is encroachment, the segmented area of the fire lane will change.
    2. **Segment Abnormal Areas** : Unlike Unsupervised Defect Segmentation models, which train using only normal data to identify anomalies and distinguish between normal and abnormal states, Supervised Defect Segmentation models can more precisely identify and segment abnormal areas and types when there are a large number of abnormal images.


Annotation Methods
------------------------

If you have a pre-trained model, you can use the assisted annotation tool to help label, then review and correct them.
    .. image:: Images/suppor_anno.png
        :scale: 100%

If the object has no defects, label it as good.

    .. image:: Images/sem_anno0.png
        :scale: 80%

If the object has defects, label it as abnormal. Use the polygon tool or intelligent polygon tool to outline the abnormal area and select the type of defect.

    .. image:: Images/sem_anno1.png
        :scale: 80%

Notes
------------

1. Images annotated as "Good" should not contain damage area annotations, as this may lead to poor training results or training failures.


Practice
-------------

Download  `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ to obtain supervised_data.zip

After extracting, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Afterward, you can upload both images and annotation files to compare the results.