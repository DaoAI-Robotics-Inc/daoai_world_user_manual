Smoke Detector Picking
----------------------

In this project, it is necessary to pick up smoke detectors, where the front and back require specific actions to successfully pick them up. Additionally, a rotating fixture corresponding to the object is needed for successful handling.

    .. image:: images/smoke.png
        :scale: 100%

After analyzing the images, we can determine:

1. Smoke detectors need to distinguish between the front and back: Two labels are required to differentiate the front and back.
2. Picking requires differentiating rotation: Key points are needed, with at least two points to determine the object's rotational orientation.

Using a key point model can meet our requirements.

    .. image:: images/smoke_label.png
        :scale: 100%

For annoating, two labels were used to distinguish between the front and back. Additionally, two relatively unique features were identified for marking key points, and only the pickable objects were annotated.

.. note::
    If key points are annotated in areas with repeated features or in regions without distinctive features, the prediction results may be poor.

After training and deployment, we can easily identify all pickable smoke detectors, including their front and back, as well as their rotational orientation.
