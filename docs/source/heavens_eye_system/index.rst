Heaven's Eye System
====================

Heaven's Eye is a high-performance intelligent video stream detection platform that supports up to **16 simultaneous** real-time video streams.
It features a built-in visual workflow engine that allows users to build detection pipelines freely by dragging and dropping components.
Multiple deep learning models and business logic modules can be flexibly combined.
Whether it's facial identity recognition or object detection, classification, or segmentation within a scene, the system enables fast deployment and one-click operation.

.. raw:: html

    <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
        <video width="80%" height="auto" controls>
            <source src="http://docs.welinkirt.com/static/videos/heavens_eye_demo.mp4" type="video/mp4">
        </video>
    </div>

|

**Core Features**:

- **Multi-Stream Concurrency**
  Supports processing up to 16 HD video streams simultaneously, meeting large-scale surveillance needs.

- **Visual Workflow**
  Comes with a built-in workflow designer, enabling flexible assembly of modules such as model loading, data preprocessing, and post-processing logic.

- **Multi-Model Support**
  - Face recognition model
  - Object detection model
  - Mixed model (integrated detection and classification)
  - Pretrained models: supports custom text labels for rapid recognition of arbitrary targets

- **Plug-and-Play**
  Easily connect various cameras without coding. The system automatically handles inference, alerts, and reporting.

- **High Scalability**
  Provides SDK/RESTful APIs for easy integration with third-party systems.

**Typical Use Cases**:

    - **Security Monitoring**
      Face recognition, stranger intrusion detection.

    - **Smart Factory**
      Detect whether workers are wearing compliant safety gear; detect fire flashes or other safety hazards.

    - **Smart Traffic**
      Detect pedestrians entering highways; identify debris or garbage on the road.


.. toctree::
    :maxdepth: 1
    :hidden:

    workflow
    faceID
    deployment
    dashboard

