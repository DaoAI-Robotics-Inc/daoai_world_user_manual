Beverage Sorting
-----------------


In this project, there are various beverages distributed across 4 boxes. We need to classify the types of beverages and locate them. As long as we know the approximate location of each object, a suction gripper can be used to pick up the items.
    
    .. image:: images/snacks.png
        :scale: 100%

After analyzing the images, we can see the following:

1. There are a total of 5 types of beverages, and there is no need to differentiate between special conditions such as orientation (front/back). Therefore, we need 5 distinct labels for classification.
2. During the picking process, there is no need to distinguish between orientations or directions (front/back). Therefore, there is no need to use keypoint models or models for directional judgment.

So, a simple instance segmentation model will be sufficient to meet our needs.

    .. image:: images/snack_label.png
        :scale: 60%

Here, we use 5 different labels, each corresponding to one of the 5 types of beverages. We annotate the visible, non-occluded surface of the beverage that is facing up using polygons. This approach focuses on annotating the parts of the objects that are accessible for picking.

.. note::
    
    Due to the variable shape of the point cloud data on the object's sides when captured with a 3D camera, and the potential for noise, the quality of detection can be affected.

    By annotating only the top surface of the object, we can focus on processing only the top point cloud during matching, avoiding the noisy side areas.

After training and deployment, we will be able to easily identify all the beverages that can be picked up.