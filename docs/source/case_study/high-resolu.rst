Instance Segmentation in High-Resolution Images
--------------------------------------------------------

In this project, our goal is to detect cattle and sheep in aerial drone images.

    .. image:: images/sheep.png
        :scale: 60%

Since aerial images usually have extremely high resolution—such as the image in this case, which has a resolution of 8192x5460 pixels—such large images pose challenges during model annotation and training.Since aerial images usually have extremely high resolution—such as the image in this case, which has a resolution of 8192x5460 pixels—such large images pose challenges during model annotation and training.


Annotating High-Resolution Images
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To address this issue, we have introduced the :ref:`Large Image Tile Annotation Mode` method, which helps you perform more precise annotations in high-resolution images.

    .. image:: images/cut_label2.png
        :scale: 60%


Simply click on the "···" in the top right corner of the annotation interface, and then select the ``Large Image Tile Annotation Mode`` from the dropdown menu to enter the tiling mode for annotation. This will allow you to efficiently divide and annotate large images.

Training High-Resolution Images
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

During training, you can use the ``高分辨率图像切片功能`` in :ref:`Preprocessing` . This feature slices large high-resolution images into smaller, lower-resolution sections. This approach preserves all the image's information without requiring compression, allowing the model to fully leverage the image data during training.
    
    .. image:: images/sheep_cut.png
        :scale: 100%


However, when slicing images, annotations that appear at the edges of the slices may get split, leading to potential issues. To address this, you can adjust the "Overlay Ratio" parameter. By increasing the overlap between slices, you can ensure the integrity of the annotations and avoid splitting important objects across slices.

    - Example of Overlay Image Regions:
        .. image:: images/sheep_overlap1.png
            :scale: 80%

    - Overlay in sliced images (showing left-right overlap only):
        .. image:: images/sheep_overlap2.png
            :scale: 80%

This preprocessing step allows us to easily train high-resolution image data, ensuring that the model performs optimally even on high-resolution images.

Model Prediction Example:

    .. image:: images/sheep_result.png
        :scale: 100%
