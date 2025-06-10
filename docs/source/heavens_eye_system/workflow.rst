Guide to Creating and Editing Workflows
=======================================

Heaven's Eye uses visual workflows to manage video stream analysis tasks. This document will guide you through creating, editing, and using workflows.

Prerequisites
-------------

.. note::
   You must have administrator privileges to use workflow-related features.

Create a Workflow
----------------------

1. Log into the Heaven's Eye system.
2. Click **"Workflow"** in the left navigation bar.
3. Click the **"Create Workflow"** button in the upper right corner, or go to the **"Template"** tab to start from an existing template.

.. figure:: images/create_workflow.png
   :width: 30%

   Figure 1: Create Workflow Page

Workflow Interface
--------------------

.. image:: images/workflow.png
    :scale: 60%

1. Back
2. Add Module
3. Rename
4. Save
5. Add a module to start
6. Zoom in, zoom out, fit to screen, lock modules

You can zoom the workflow using the scroll wheel, or drag with the mouse.

Modules
---------------

Modules are divided into 6 categories, as shown below:

.. image:: images/nodes.png
    :scale: 80%

Add a Module
~~~~~~~~~~~~~

Click "Add Module" in the top-left, then click on a module to add it.
For detailed descriptions of each module, see below.

Delete a Module
~~~~~~~~~~~~~~~~

Click to select a module, then click the **minus** icon on the right to delete it.

.. image:: images/delete_node.png
    :scale: 80%

Reconnect Modules
~~~~~~~~~~~~~~~~~~

Click to select a connection line, then click the **minus** icon on the right to delete the link between modules.

.. image:: images/delete_edge.png
    :scale: 80%

Click the black dot at the bottom of a module, then drag to another module to connect them.

.. image:: images/reconnect_nodes.png
    :scale: 70%

Modules execute in top-down order following the connections.

Model Modules
~~~~~~~~~~~~~~~~

.. image:: images/node_models.png
    :scale: 80%

1. :ref:`Object Detection` Module: Select a trained model from a DaoAI platform project. This module performs object detection inference on video stream frames.

.. image:: images/add_model.png
    :scale: 60%

.. image:: images/import_model.png
    :scale: 60%

2. :ref:`Mixed Model` Module: Select a trained model from a DaoAI platform project. This module performs both object detection and classification on video frames.

3. **Face Detection Module**: This module is pre-trained and can be used without training a new model.
   You must first register face identities in the face identity database. For details, see :ref:`Register Face Identity`.

4. **Generic Object Detection**: This module is pre-trained and does not require training.
   You only need to enter the labels you want to detect. The model will identify corresponding objects based on semantic matching.

    .. image:: images/generic_od.png
        :scale: 70%

.. note::
    After creation, the system will automatically enter the workflow editing interface.

Logic and Branching
~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/logic_nodes.png
    :scale: 100%

1. **Parallel Branch**: Runs two model inference threads in parallel. After both models complete, the results are merged before continuing with the next module.

    .. image:: images/parallel_node.png
        :scale: 100%

    .. note::
        By default, the workflow runs in a single thread. If no parallel branch is added, modules execute sequentially. With a parallel branch, two modules run concurrently.

2. **Conditional Continue**: Compares the output of a counter module with a target value. If the condition is met, the workflow continues. You must select the counter node, a comparator, and a threshold value.

    .. image:: images/if.png
        :scale: 100%

Transformation
~~~~~~~~~~~~~~~~~

.. image:: images/transformation.png
    :scale: 100%

1. **Region of Interest (ROI)**: Limits detection results to within a defined ROI. Predictions outside this region are filtered out. See :ref:`Device Management` for how to configure ROIs.

    .. note::
        The ROI module must be the first module in the workflow for it to take effect.

Visualization
~~~~~~~~~~~~~~~~~

.. image:: images/visualize.png
    :scale: 100%

1. **Bounding Box Visualization**: Select a model output or object relationship analysis result to overlay bounding boxes on the video frames.
   To display it on the large screen, you must also use the **Dashboard Notification** module.

    .. image:: images/visual.png
        :scale: 100%

Notification
~~~~~~~~~~~~~~~~~

.. image:: images/notification.png
    :scale: 100%

1. **Dashboard Notification**: Sends selected messages or visualization overlays to the central monitoring dashboard. You can send:
   1. Custom messages
   2. Default model outputs
   3. Blank messages

   You can also configure the data retention time (from 1 day to 365 days).

    .. image:: images/noti.png
        :scale: 100%

Example Workflows
----------------------

1. Mixed Model: Detecting People Not Wearing Helmets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/template_1.png
    :scale: 100%

This template detects the number of people not wearing helmets.

1. Mixed Model Module: Uses a trained mixed model to detect people and classify whether they are wearing helmets.

2. Count Module: Counts the number of people without helmets.

3. Conditional Continue: If the count is greater than or equal to 1, it proceeds to the next step. Otherwise, it stops and moves on to the next frame.

4. Bounding Box Visualization: Draws detection boxes from the mixed model and overlays them onto the current frame.

5. Dashboard Notification: Sends an alert to the monitoring dashboard, e.g., “Person not wearing helmet detected”.

2. Generic Object Detection Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/template_2.png
    :scale: 100%

This is a template workflow for generic object detection. You can define the detection targets using semantic input. No training is required, making deployment very flexible.

1. Generic Object Detection: You can input any object label in English to define the detection target.
   No additional training is required. The model will automatically detect the object based on semantic understanding.

2. Counting Module: Counts the results from the generic object detection.

3. Conditional Continue: If the count is greater than or equal to 1, the workflow continues.
   Otherwise, it stops and moves on to the next frame.

4. Bounding Box Visualization: Draws the model’s detection boxes and overlays them onto the current frame.

5. Dashboard Notification: Sends an alert message to the monitoring dashboard, such as “Target detected”.

3. Object Detection Template for Litter Detection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. image:: images/template_1.png
    :scale: 100%

This template detects litter and can be applied to any general object detection use case.

1. Object Detection Module: Uses a trained object detection model to identify litter.

2. Counting Module: Counts the amount of detected litter.

3. Conditional Continue: If the count is greater than or equal to 1, the workflow continues.
   Otherwise, it stops and proceeds to the next frame.

4. Bounding Box Visualization: Draws the detection boxes from the model and overlays them onto the current frame.

5. Dashboard Notification: Sends an alert to the monitoring dashboard, such as “Litter detected, please clean promptly”.
