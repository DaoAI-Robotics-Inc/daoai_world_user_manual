Assembly Detection
-------------------

In this project, we need to verify whether the assembly of the following parts is correct by checking if all required screws are present. Specifically, we need to detect if there are 4 screws in total.
    .. image:: images/shanghengliang.png
        :scale: 100%


After analyzing the images, we can determine that:

1. There are several screw holes in the image, some of which are missing screws. We need to detect both the presence of screws and their quantity.

An object detection model can fulfill our requirements. 

    .. image:: images/shanghengliang_label.png
        :scale: 70%
        
By using bounding boxes to highlight the holes without screws, we can effectively identify missing screws.

Once trained and deployed, the model will allow us to easily recognize which holes are missing screws and determine how many screws are not installed.