Monitoring Dashboard
=============================

The DaoAI Heaven’s Eye system provides a unified monitoring dashboard interface, integrating core modules such as performance overview, event alerts, live video feeds, face recognition library, and device management. It helps users quickly understand site operations and perform efficient maintenance.

Section 1: Performance Overview
-------------------------------

**Feature Description**

The performance page provides system administrators with a global view of alert statistics, resource usage, and workflow popularity—useful for troubleshooting and trend analysis.

    .. figure:: images/dashboard.png
        :alt: Monitoring Dashboard Interface
        :align: left
        :width: 90%
    
        Figure 1: DaoAI Heaven’s Eye System Dashboard Overview

**Functional Areas**

- **Total Alerts**: Total number of intelligent analysis alerts triggered by the system.

- **Alert Trend Chart**: View alert curves by “Today,” “Last 7 Days,” “Last 14 Days,” or “Last Month.”

- **Live Event Preview**:
  - Alert snapshots (multi-frame burst)
  - Event time, camera ID, alert type (e.g., not wearing safety helmet)

- **Top Workflows**:
  - Displays the most frequently triggered models (e.g., ``facedetected``, ``helmet detection``)

- **System Resource Monitoring**:
  - CPU usage
  - Memory/GPU memory usage
  - Disk usage

**Helpful Tips**

- If alerts are triggered too frequently, check your deployment or adjust model sensitivity.
- If resource usage exceeds 85%, consider scaling out or redistributing tasks.

Section 2: Live Video
----------------------

**Feature Description**

This module supports viewing real-time video streams from deployed cameras for field review, event confirmation, and demonstrations.

    .. figure:: images/stream.png
        :alt: Real-Time Video Preview
        :align: left
        :width: 90%

        Figure 2: DaoAI Heaven’s Eye System Real-Time Preview Page

**Operation Steps**

1. Open the bottom navigation bar and click **Live Video**
2. Click the blank area and select ``Choose a camera``
3. Use the dropdown menu to switch between different devices for preview

**Notes**

- Real-time video is displayed with low latency; if playback is choppy, check your network and system resource usage.
- Supports up to four simultaneous camera views per page. For more, use a professional client or video wall module.

Section 3: Camera Device Management
-----------------------------------

**Feature Description**

This module displays all connected camera devices and supports basic operations such as enabling/disabling and renaming.

    .. figure:: images/设备管理.png
        :alt: Camera Device Management
        :align: left
        :width: 90%

        Figure 3: DaoAI Heaven’s Eye System Device Management Page

**Key Fields**

- Camera Name  
- Camera ID (used for event tracking)  
- Status Indicator:  
  - Blue = Active/Streaming  
  - Gray = Inactive/Disabled

**User Guide**

- Use the toggle button to quickly enable or disable cameras

Section 4: Face Recognition Library
-----------------------------------

**Feature Description**

The DaoAI Heaven’s Eye system supports recording employee photos through the “Face Recognition Library” module. It extracts facial feature vectors for identity verification, attendance tracking, access control, and other use cases in workflows.

    .. figure:: images/人脸识别.png
        :alt: Face Recognition Library
        :align: left
        :width: 90%

        Figure 4: DaoAI Heaven’s Eye System – Face Recognition Library Overview

Register New Profile
^^^^^^^^^^^^^^^^^^^^

Click **Face Recognition** in the left menu to access the face library management module. Click **Register New Profile** to open the registration interface:

Field Descriptions
^^^^^^^^^^^^^^^^^^

- **Name/ID** (Required): Enter a unique identifier for the employee (e.g., name, employee ID)
- **Notes** (Optional): Used to describe team, role, etc.
- **Photo** (Required): Supports the following image upload methods:

  1. Drag and drop a face image into the upload box
  2. Click **Select File** to upload from your local computer
  3. Or click **Use Camera** to take a photo using a USB camera

Photo Requirements
^^^^^^^^^^^^^^^^^^

To ensure accurate recognition, the uploaded photo must meet the following standards:

**Image Quality**

- Must be clear and in focus; no blurriness
- Supports both color and grayscale images
- Background must be clean, without glare or shadows
- Avoid direct sunlight or backlighting; ensure good contrast between face and background
- Face should be directly facing the camera, showing full facial features

Important Notes
^^^^^^^^^^^^^^^

- Each user only needs to register once; the system will automatically extract and store the facial feature vector
- Using a high-definition camera is recommended to improve recognition accuracy
- To delete a registered user, return to the face library main page and click the user’s avatar to manage their record

Section 5: Device Deployment Status
------------------------------------

Under **Deployment > Camera Devices**, you can view the following for each camera:

- Status (Running / Stopped)
- Assigned Workflow
- Video Stream URL
- Alert Notification Interval
- ROI (Region of Interest) settings and preview

Section 6: Common Issues and Recommendations
--------------------------------------------

- Real-time video not displaying?
    Check RTSP configuration, network connectivity, and camera power supply.

- Too many alerts?
    Adjust model sensitivity or narrow the region of interest.

- Model not responding after deployment?
    Check if the container is running and if the workflow is properly configured and assigned to the device.

- Resource usage too high?
    Distribute tasks more efficiently or consider expanding server capacity.
