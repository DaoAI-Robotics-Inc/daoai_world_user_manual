Release Notes
===================

.. contents:: 
    :local:

Version 2.24.7.0 Updates:
------------------------------------

- Brand New Project Create page.
- Added Demo Projects for guidance and demo purpose.
- Image Display Setting allows users to adjust image display settings during annotation to improve annotation efficiency.
- View annotations and detection results at the same time allowing for better distinction between the two.
- New Positioning project for detecting objects in images and marking their positions, making it suitable for various industrial applications such as robotic assembly.
- New Presence Detection project for detecting the installation status of parts by comparing the image with a reference image. It is commonly used for detecting misassembled or missing parts.
- New classification project annotation image as background image function, indicating that there are no other objects in the image, only the background.
- New OCR dictionary function which allows focus on the language of text recognition projects. Users can select the desired language for recognition, improving efficiency and accuracy.
- New decision criteria function, allowing users to select the criteria that best align with the model's requirements. This helps to better filter detection results during model inference.
- Image Argumentation Times allowing users to adjust the number of image augmentations to create more training samples. Larger versions will take longer to train but typically result in better model performance.
- Train from Previous Checkpoint feature, it is recommended to start from a previous training run to speed up the training process and improve accuracy. If user have successfully trained a model on this project before, this option is the best choice.
- Inference Information Display shows inference time and the type of GPU used for training. This helps users better understand the recognition speed.

Detailed information can be viewed in the Change Log at the top right corner of our website.

.. image:: images/changelog.png
    :width: 800
    :align: center
