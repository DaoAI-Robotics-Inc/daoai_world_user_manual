Presence Detection
==========================================

**Presence Detection** can detect the presence of parts in images by comparing them with standard images. Commonly used for checking for misplacement and missing parts.

    .. image:: Images/pre.png
        :scale: 100%

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.

Use Case Scenarios
------------------------------------------

**Presence Detection** determine whether a specific object appears in a scene or to count the number of times the object appears.

**Presence Detection** requires a golden image as the learning target, and it will learn to determine the number of times the object appears.


Annotation Methods
-----------------------

If you have a pre-trained model, you can use the assisted annotation tool, allowing the deep learning model to help with annotations. You can then verify and correct the annotations as needed.

    .. image:: Images/suppor_anno.png
        :scale: 100%

Use the bounding box tool(green) or smart polygon tool(red) to annotate the outer contours of the objects.

    .. image:: Images/pre_annotate.png
        :scale: 75%

Presence Detection model requires a **golden image** as the standard. You need to set one golden image, then finish setting up layout; otherwise, the dataset cannot be fully annotated.

    .. image:: Images/pre_golden_image.png
        :scale: 75%

Click ``Save as Golden Image`` , then click ``continue``.

    .. image:: Images/pre_golden_continue.png
        :scale: 100%

.. warning::
    ``Save as Golden Image`` will reset your previous golden image and layout. Please make be aware.

Select layout box, and annotate the layout area.

    .. image:: Images/pre_layout.png
        :scale: 75%

The layout can be the overall extent of the workpiece or the ROI (Region of Interest) where the objects may appear.   

    .. image:: Images/pre_golden_board.png
        :scale: 75%

After saving, you can view the current layout labels, including the object labels and their quantities within the layout.

    .. image:: Images/pre_golden_board_save.png
        :scale: 75%

Repeat the process to annotate all the objects.

    .. image:: Images/pre_annotate_all.png
        :scale: 75%

If there is no object in the scene, please mark it as null.

    .. image:: Images/pos_null.png
        :scale: 100%

Note
------------

1. When annotating **Presence Detection** , you should annotate the golden image and setup the layout first.

2. The annotation region for the **Presence Detection** must not exceed the image boundaries.

3. As with other annotation models, avoid annotating objects that are heavily obscured. Instead, focus on annotating the topmost or most visible objects.

Practice
----------
Download the `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ with presence_checking.zip


After unzipping, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Later, you can upload both images and annotation files to compare the results.