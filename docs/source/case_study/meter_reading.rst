Meter Reading
-------------

In this project, the task is to detect the readings of a meter from images captured from different angles.

    .. image:: images/meter.png
        :scale: 100%


After analyzing the images, we can determine that:

1. To accurately read the pointer’s position on the meter, you’ll need a model that provides precise location data, such as instance segmentation or keypoint detection.
2. Since only the pointer’s position needs to be determined and there are no special orientations (e.g., front/back), a single label is sufficient for this task.


Since keypoint detection involves more complex annotations, we will use an instance segmentation model for this task.

    .. image:: images/meter_label.png
        :scale: 60%


Since keypoint detection involves more complex annotations, we will use an instance segmentation model for this task.

After training and deployment, we will be able to easily identify the pointer's position in the scene. We can then use other detection algorithms to determine the pointer’s angle and reading.