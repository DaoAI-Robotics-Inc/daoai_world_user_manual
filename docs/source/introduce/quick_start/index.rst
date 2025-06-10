Quick Start
=================

.. tabs::
    DaoAI World is designed to seamlessly interpret images into valuable information, offering a comprehensive suite of computer vision tools. |br|
    From data annotation to model training and deployment, 
    our platform provides an end-to-end solution that enables you to accomplish your goals efficiently. |br|
    Whether you're new to machine learning or an experienced professional, 
    DaoAI World makes it easy to get started, ensuring you can quickly master the platform. 

|

.. raw:: html

    <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
        <video width="80%" height="auto" controls>
            <source src="http://daoai-robotics-1305756387.file.myqcloud.com/videos/DaoAI_World_Demo.mp4" type="video/mp4">
        </video>
    </div>

|


In this guide, we will train a keypoint detection model to identify the outer contour of door handles, their orientation (front or back), and corresponding keypoints.  |br|
This tutorial will help you quickly get started with DaoAI World, and the knowledge gained from this project can also be applied to other tasks.


Select Language
---------------------

After opening the DaoAI World homepage, you can select your preferred language from the top-right corner, with options available in both Chinese and English.

    .. image:: images/signin_screen.png
        :width: 800
        :align: center


Login/Register
---------------------

DaoAI World includes a user management feature, where each user has their own independent workspace.  |br|
This ensures data security and prevents project mix-ups. 

If you don’t have a DaoAI World account yet, you can click the "Create one" option to create a new one.  |br|
If you already have an account, simply select "Login" to access your workspace.


    .. image:: images/signin_screen.png
        :width: 800
        :align: center


Create a Project
---------------------

After logging into your account, you will see the project management page.  |br|
You can create a new task by setting up a new project. Each project operates independently, and you can rename each project as needed. 

    .. image:: images/create_project.png
        :width: 800
        :align: center

Now, let's create our first project.

Under **Project Type** , select the type of project you want to create. In this example, we will choose **Keypoint Detection** , add a project name, and then click **Create Project** .
    
    .. image:: images/project_init.png
        :width: 800
        :align: center

For more detailed information about the features of each model, please refer to the :ref:`Select a Model` section.

Next, we need to configure the label information for the data. If the data you are going to add are already annotated, you can select **Skip and Create Empty Project** ;

If the data is not already annotated, you can add label information here. Once the label information is added, you can click **Create Project** to proceed.

    .. image:: images/create_label.png
        :width: 800
        :align: center

**Demo Project**

    .. image:: images/demo_project.png
        :scale: 80%
        :align: center

DaoAI World can use demo project's data as guidance or for copying into your own projects for practice.

    .. image:: images/demo_copy.png
        :scale: 80%
        :align: center

Click on the project you wish to select, you will see an introduction to the project. You can then ``duplicate this project`` it to your own workspace for use.

    .. image:: images/demo_copied.png
        :scale: 80%
        :align: center

|

**Upload Data**

DaoAI World supports uploading single images (png, jpg, jpeg, bmp) or uploading all images from a folder. You can also upload annotated data files (.json) together with the images, or directly import data from other pre-existing projects.

Here, we will click on **Select Folder** to upload our dataset in bulk.

    .. image:: images/upload_folder.png
        :width: 800
        :align: center

In the pop-up file selection window, choose your dataset folder and click on **Select Folder** . Wait for the data to finish uploading.

    .. image:: images/select_folder.png
        :width: 800
        :align: center

Then, you will see all your images displayed in the window. You can modify the dataset name or delete individual images if needed, but we won't demonstrate that here. 

If everything looks correct, you can **click Save and Continue** to upload the data to the project and proceed to annotation.
    
    .. image:: images/continue.png
        :width: 800
        :align: center

Data Annotation
---------------------

    + **Polygon Annotation**

        The image below shows an unannotated picture from the protractor dataset. You will use the polygon tool to outline the protractor's outer contour.  |br|
        The information you annotate serves as the ground truth answer the computer vision model will learn from. The more images we have and the more annotations we create,  |br|
        the more information the model will have to learn the characteristics of each class. 

        Follow the polygon tool instructions to annotate the unmarked protractor in the image below.

            .. image:: images/annotation.png
                :width: 800
                :align: center

        Use the **Smart Polygon Tool** to mark the object's outer contour. 
        
        Select the **Smart Polygon Tool** , then move the cursor over the object you want to label.  |br|
        Observe the tool's suggested boundary around the object. If you're satisfied with the current boundary, click the left mouse button to confirm the polygon outline.  |br|
        You can also use the right mouse button to select the parts where you want to exclude from the boundary.

            .. image:: images/annotated.png
                :width: 800
                :align: center

        We have just added an "mask" annotation to the image. This indicates that we have outlined the object of interest.  |br|
        Adding an outer contour is a common technique in computer vision, frequently used for segmentation tasks.  |br|
        The polygon tool helps you accurately trace the precise outline of the object of interest.

    + **Keypoint Annotation**

        Since our current project type is keypoint annotations, in addition to annotating the object's mask, we also need to add keypoint data.  |br|
        This will enable the computer vision model to recognize the precise position and rotation information, making it easier for robots to perform high precision tasks such as grasping or other operations.

        Once we have completed the outer contour annotation of an object, the system will prompt us to annotate the keypoints for the current object. Click **Start Keypoint Annotation** to begin annotating the keypoints.

        .. image:: images/kp_notice.png
            :width: 800
            :align: center

        Click on the location of the keypoint on the object, then select the corresponding keypoint name from the list on the right. Click **Save** or press ``Enter`` to complete the annotation of a keypoint.

        .. image:: images/kp_label1.png
            :width: 800
            :align: center
        
        Repeat the above steps until all keypoints for the current object are annotated. Once all keypoints are annotated, click Complete to **Finish** annotating the entire object.

        .. image:: images/kp_label2.png
            :width: 800
            :align: center

+ **Add Annotated Data to Dataset**

    Once you have annotated all your images, you need to add the annotated data to your dataset for model training.  |br|
    Go to the annotation interface and click on **Add Annotated Images to Dataset** in the top right corner to add your annotated data to the dataset for training.

    .. image:: images/add2dataset.png
        :width: 800
        :align: center


+ **Create Dataset Version**
    After completing the annotations and adding the annotated images to the dataset, click **Generate** to generate a new version of the dataset (you can think of it as version control for the data).

    .. image:: images/create_train_set.png
        :width: 800
        :align: center
    
    Before creating the training dataset, we need to use the built-in dataset health check tool to confirm whether there are any annotation issues in the dataset that might cause training failures.  |br|
    Click **Run Health Check** and wait for the dataset report to be generated.

    .. image:: images/healthcheck.png
        :width: 800
        :align: center

    You can also choose functions such as preprocessing and data augmentation.  |br|
    For convenience, preprocessing and data augmentation have not been added here (it’s important to note that more preprocessing and data augmentation do not necessarily lead to better model; only the appropriate methods are beneficial). 

    After making your choices, click Create to complete the creation of a dataset version.

    DaoAI World provides default preprocessing methods for some training models, as shown in the image below:
    
    .. image:: images/default_preprocess.png
        :width: 800
        :align: center

    For more information on preprocessing and data augmentation methods and their effects, please refer to :ref:`Dataset Management` .

Training
-----------------------

    After completing the above steps, we arrive at the final step: the training phase. 
    
    DaoAI World offers various training modes to meet different model application needs.  |br|
    For keypoint detection projects, DaoAI World provides three training modes: **Fast** , **Accurate** , and **Rotation Accurate** .  |br|
    You can choose the training mode that best suits your needs.

    .. image:: images/training_mod.png
        :width: 800
        :align: center

    After making your selection, click Continue and Select the label and checkpoint for this training. 

    .. image:: images/training.png
        :scale: 70%
        :align: center
    
    .. image:: images/train_checkpoint.png
        :scale: 70%
        :align: center

    Then select the **Max Augmentation Size** , then click **Generate and Start Training** to start training.
    
    .. image:: images/training_max_size.png
        :width: 800
        :align: center
    
    Once training starts, you can view the real-time model training progress. The required training time will also be displayed above the chart.

    .. image:: images/training_2.png
        :width: 800
        :align: center

    During the model training process in DaoAI World, you can monitor the training status by viewing the real-time training charts.  |br|
    Once DaoAI World has completed the model training, it will look like the image below. 
    
    You can then choose to export the model. The exported model can be used with `DaoAI Vision Pilot <http://docs.welinkirt.com/daoai-vision-system-user-manual/chinese-2.24.4.0/index.html>`_ , `DaoAI InspecTRA <http://docs.welinkirt.com/daoai-inspectra-user-manual/chinese-2.24.4.0/index.html>`_ , 
    or the DaoAI World SDK. 
    
    .. image:: images/training_over.png
        :width: 800
        :align: center

    You can also click on Visualize in the left navigation bar to manually check the model's result accuracy. Or inspect the Model's performace on the dataset.

    .. image:: images/deploy_test.png
        :width: 800
        :align: center

.. |br| raw:: html

      <br>