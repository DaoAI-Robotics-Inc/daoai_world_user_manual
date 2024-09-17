Select a Model
=============================

.. toctree::
    :maxdepth: 1
    :hidden:

    seg/index
    kp/index
    ano/index
    class/index
    obj/index
    rot_obj/index
    semantic/index
    ocr/index
    playground/index


DaoAI World supports the following models:

    - :ref:`Instance Segmentation`
    - :ref:`Keypoint Detection`
    - :ref:`Anomaly Detection`
    - :ref:`Image Classification`
    - :ref:`Object Detection`
    - :ref:`Semantic Segmentation`
    - :ref:`OCR`

**How to Choose a Model?** Different models have their own unique features and best-use cases. Here are some points to help you select the right model:

- Detecting objects in an image:

    - :ref:`Object Detection` : Detects the **type** and **bounding box** of an object. Use this model when you need to detect the presence of objects in an image. Example scenarios: detecting the number of vehicles on the road, checking if screws are installed or missing in a part, detecting whether workers are wearing helmets.
        
        .. image:: images/obj.png
            :scale: 100%

    - :ref:`Instance Segmentation` : Detects the **type** and **mask** of an object. Use this model when you need to detect the presence of objects and their masks. Example scenarios: identifying the position and area of a package, locating parts inside a box, or classifying different types of screws in one image.
        
        .. image:: images/insseg.png
            :scale: 100%
            
    - :ref:`Keypoint Detection` : Detects the **type** , **mask** , and **keypoints/pose** of an object. Use this model when you need instance segmentation with additional keypoint detection. Example scenarios: detecting the posture of athletes, determining the current position and rotation of an object.
        
        .. image:: images/kp.png
            :scale: 100%
            
- Image Classification:    
    - :ref:`Image Classification` : Detects the **category** of an image. Use this model when you need to classify images. Example scenarios: classifying product models, checking if specific screw holes are installed correctly, diagnosing diseases from X-rays.
        
        .. image:: images/class.png
            :scale: 100%

- Detecting anomalies or surface defects on objects:
    
    - :ref:`Anomaly Detection` : Detects the **mask** of abnormal areas in an image. Use this model when you need to detect abnormal regions in an image. Example scenarios: detecting surface defects on parts, soldering defects, scratches on lithium batteries.
        
        .. image:: images/ano.png
            :scale: 100%

    - :ref:`Semantic Segmentation` : Detects the **type** and **mask** of abnormal areas in an image. Use this model when you need to detect and classify abnormal areas in an image. Example scenarios: detecting multiple types of anomalies on part surfaces, ensuring components on a chip are installed correctly.
        
        .. image:: images/sem.png
            :scale: 100%

- Detecting text in images:

    - :ref:`OCR` : Detects **text** in images. Use this model when you need to extract text, letters, or symbols from an image.
        
        .. image:: images/ocr.png
            :scale: 100%