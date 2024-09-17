Lenses Quality Control
-----------------------

In this project, our goal is to detect the condition of the lenses, assess their quality, and identify any defects.

    .. image:: images/lens.png
        :scale: 100%


After analyzing the images, we can determine that:

1. The lens positions are consistent, but some images may show defects. For this, we can use anomaly detection or semantic segmentation models to identify and analyze the defects.

Since we don't need to differentiate between types of defects, a defect detection model will meet our needs effectively.

    .. image:: images/lens_bad.png
        :scale: 70%

    .. image:: images/lens_good.png
        :scale: 80%
        
Annotate the defect regions using polygons, or label the image as "no defect".

.. note::
    
Anomaly detection models are trained using images of normal conditions. Therefore, it's essential to ensure that more than half of the images in the dataset are labeled as "no defect". This helps the model learn a clear baseline of what constitutes a "no defect" lens and effectively identify deviations.

After training and deployment, we will be able to easily identify the lens condition, assess whether the quality meets standards, locate any defects, and more.