Mixed Model
==========================================

**Mixed Model** is similar to object detection but goes further by classifying the detected targets into more specific categories.

    .. image:: Images/mix_model.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/daoaiworld_mixed_model.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.

Use Case Scenarios
------------------------------------------

**Mixed Model** is suitable for determining whether an object appears in a scene and identifying its category. It is well-suited for smart surveillance and detecting risky or hazardous objects.

It can also be used in defect detection scenarios to identify multiple objects in a scene and classify them as OK or NG.

Label Creation
-------------------

When creating labels for a mixed model, refer to the following structure:

    .. image:: Images/create_label.png
        :scale: 80%

- The **first level** is the **object category**
- The **second level** is the **subcategory**
- The **third level** is the **attribute** of the subcategory

.. warning::

   When annotating the third-level "attribute", ensure that each **first-level category** has at least one attribute selected.
   For example, for the category "Head", subcategories might include "Helmet Color" (red/blue/black) and "Wearing Mask" (yes/no).

   - Attributes like "Helmet Color" and "Wearing Mask" can coexist if they do not logically conflict.
   - If there’s a conflict (e.g., between "Helmet Color" and "Wearing Helmet"), then when "Not Wearing Helmet" is selected, no helmet color should be labeled.
   - Make sure each subcategory has one and only one valid attribute selected, and subcategories do not conflict with each other.

Annotation Method
-------------------

Use the rectangle annotation tool to draw a bounding box and select the main category.

    .. image:: Images/mix_anno1.png
        :scale: 80%

Click save, then select the subcategory and its attribute.
**Note**: If the main category has multiple subcategories, each must have one and only one valid attribute selected.

    .. image:: Images/mix_anno2.png
        :scale: 80%

Repeat the annotation process for all objects in the scene. If there are no objects, annotate the image as empty.

Notes
--------------

1. The **Mixed Model** only supports rectangular annotations. Therefore, annotation areas may slightly overlap but should not fully overlap.

2. Annotation areas must not exceed image boundaries.

3. If a category includes multiple subcategories, each must have exactly one valid attribute selected.

Practice
----------

Download and unzip `mixed_model.zip` from `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_.

You will get 21 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Later, you can upload both the images and annotation files to compare the results.
