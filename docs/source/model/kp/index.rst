Keypoint Detection
=================================

In **Keypoint Detection** , the model learns the object's masks and keypoint positions to identify the object's category and its orientation and position.
    
    .. image:: Images/kp.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_kp-v6.mp4" type="video/mp4">
            </video>
        </div>

|

After completing the model annotations, refer to the video in the :ref:`Training` section to create dataset versions and train/deploy the model.


Use Case Scenarios
---------------------------------

**Keypoint Detection** models can be used to detect and precisely locate one or more objects in an image.

Similar to **Instance Segmentation** models, Keypoint Detection models can segment and locate one or more objects. 
However, unlike Instance Segmentation, Keypoint Detection uses keypoints for more precise localization, providing accurate information about the object’s position, orientation, and rotation.

Annotation Method
---------------------------

If you have a pre-trained model, you can use the assisted annotation tool, allowing the deep learning model to help with annotations. You can then verify and correct the annotations as needed.
    
    .. image:: Images/suppor_anno.png
        :scale: 100%

Keypoint models require the definition of an outer contour mask and the keypoint structure.

    .. image:: Images/kpAnno0.png
        :scale: 100%

Use polygons or intelligent polygons to annotate the object's outer contour.

    .. image:: Images/kpAnno2.png
        :scale: 100%

After completing the contour annotation, the system will automatically switch to keypoint annotation mode. You need to click on each keypoint’s location to annotate them.

    .. image:: Images/kpAnno5.png
        :scale: 100%

Repeat this process for all objects in the scene. If no objects are present in the scene, annotate it as empty.

    .. image:: Images/kpAnno6.png
        :scale: 100%

While annotating, ensure that the keypoints' positions and order are relatively fixed for optimal model performance.

Notes
------------------

1. Use descriptive labels when naming the tags. Descriptive labels significantly reduce the likelihood of annotation errors and facilitate the practical application of the model. Non-descriptive labels are loosely connected to the annotated object, increasing the chance of mistakes and making it harder to quickly determine the accuracy of model predictions.

2. Each tag group in keypoint detection must include a polygon and one or more keypoints. If there are no keypoints in a tag group, the training task may fail.

3. For each annotated polygon, there should be at least 3 keypoints associated with that label. When choosing keypoints, select ones that are representative of the object and easy to identify. Geometric features like circular points, corners, or the center of an object are ideal choices. In general, look for geometric or texture features, or any other shape or pattern characteristics. Avoid selecting flat points with no special features or feature points that are too close to the polygon's edges. Also, avoid using keypoints that form a straight line, as this can reduce the ability to recognize tilt angles.

4. Do not annotate objects whose keypoints are obscured by other objects, as the missing keypoints in the tag-keypoint combination will cause the training to crash.

5. Similar to segmentation, only annotate the top-layer keypoint-tag set in each image and avoid annotating objects that are obscured by other objects.


Practice
--------

Download the `practice data <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ with keypoint_detection.zip.

After unzipping, you will get 11 images and annotation (.json) files. Please upload only the images to DaoAI World for annotation practice. Later, you can upload both images and annotation files to compare the results.