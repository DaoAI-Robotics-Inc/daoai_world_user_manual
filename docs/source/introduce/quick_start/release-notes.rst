Release Notes
=============

.. contents::
    :local:

Version 2.25.3.0 Update Notes
-----------------------------

- Updated the rotated object detection model
- Updated the hybrid model
- Updated the Sky Eye system and the video stream detection system
- Fixed several issues that could cause training failures

Version 2.25.2.0 Update Notes
-----------------------------

- Updated ultra-high-resolution training mode for supervised defect segmentation; now supports training with images resized up to 3584×3584 resolution
- Updated pixel-level training for unsupervised defect segmentation: now only one normal sample is needed to train and detect unknown defects effectively; removed the 300-image dataset limit
- Removed the full-image mode in unsupervised segmentation; now all detection is region-based. For full-image detection, draw the detection region as the entire image

Version 2.25.1.0 Update Notes
-----------------------------

- Updated SDK encryption method: all SDKs now require a hardware dongle for use
- Fixed issues causing training failures in some models
- Added support for local HTTP inference in C#
