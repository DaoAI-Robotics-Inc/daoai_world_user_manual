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


In this guide, we will train a keypoint detection model to identify the outer contour of door handles, their orientation (front or back), and corresponding keypoints. 
This tutorial will help you quickly get started with DaoAI World, and the knowledge gained from this project can also be applied to other tasks.


Select Language
---------------------

After opening the DaoAI World homepage, you can select your preferred language from the top-right corner, with options available in both Chinese and English.

    .. image:: images/signin_screen.png
        :width: 800
        :align: center


Login/Register
---------------------

DaoAI World includes a user management feature, where each user has their own independent workspace. 
This ensures data security and prevents project mix-ups. 

If you don’t have a DaoAI World account yet, you can click the "Create one" option to create a new one. 
If you already have an account, simply select "Login" to access your workspace.


    .. image:: images/signin_screen.png
        :width: 800
        :align: center


Create a Project
---------------------

After logging into your account, you will see the project management page. 
You can create a new task by setting up a new project. Each project operates independently, and you can rename each project as needed. 

    .. image:: images/create_project.png
        :width: 800
        :align: center

Now, let's create our first project.


在 **项目类型** 中选择想要创建的项目类型，这里我们以关键点检测项目为例，选择 **关键点检测** ,并添加项目名称，随后，点击 **创建项目** 。

    .. image:: images/project_init.png
        :width: 800
        :align: center


    .. image:: images/more_project.png
        :width: 800
        :align: center

更多关于模型特点的详细信息，请参阅 :ref:`模型`。

随后，我们需要设定数据中的标签信息，如果我们添加的数据中已包含标签，则可以选择 **跳过并创建空项目** ；如果数据未经标注，则可以在此处添加标签信息，标签信息添加结束后，即可点击 **创建项目** 。
    .. image:: images/create_label.png
        :width: 800
        :align: center

**上传数据**

DaoAI World支持上传单张图片(png, jpg, jpeg, bmp)、或者上传一整个文件夹中的所有图片。也可以连带图片的标注数据(.json)文件一起上传，或者直接导入其它创建好的项目中的数据。

这里我们点击 **选择文件夹** 来批量上传我们的数据集。

    .. image:: images/upload_folder.png
        :width: 800
        :align: center

在弹出的文件选择框中选择我们的数据集文件夹，点击 **选择文件夹** ，等待数据上传完成。

    .. image:: images/select_folder.png
        :width: 800
        :align: center

然后我们就可以看到我们所有的图片都已经在审查界面中中，我们可以修改数据集名称，或者删除单张数据等操作，这里就不做演示，如果检测发现没有问题，接下来就可
以点击 **保存并继续** 将数据上传到项目中，等待标注。

    .. image:: images/continue.png
        :width: 800
        :align: center

数据标注
---------------------

    + **外轮廓标注**

        下图所展示的是调角器数据集中未标注的一张照片，您将使用多边形工具对调角器的外轮廓进行标注。您所标注的信息就是计算机视觉模型所要学习的知识的答案。我们的
        图像越多，标注的数据越多，计算机视觉模型就有越多的信息来学习每个类是什么。(在这里例子中，更多的标注信息将更好的帮助我们模型识别什么是调角器，以及调角器
        的正反面。)

        按照多边形工具的说明将未标注的调角器进行标注。您可以在下面图像上进行标注。


            .. image:: images/annotation.png
                :width: 800
                :align: center

        使用 **智能多边形工具** 标记处物体的外轮廓。选择 **智能多边形工具** ，然后将鼠标移动到想要标记的物体上，观察智能多边形工具的提示，如果对当前边界满意，点击鼠标左键确认多边形边界。也可以使用鼠标右键来撤销部分区域的选择。

            .. image:: images/annotated.png
                :width: 800
                :align: center

        我们刚刚为图像添加了一个"外轮廓"注释。这表明我们在感兴趣的对象周围添加了一个外轮廓。添加外轮廓是计算机视觉中常用的方法，常常被用于分割项目。多边形工具能帮助您绘制
        感兴趣对象的精确轮廓。


    + **关键点标注**

        由于我们当前的项目类型为关键点项目，因此在外轮廓标注之外，我们还需要添加关键点数据，让计算机视觉模型能够识别相应的点位，从而让机器人能够更方便的进行抓取或其他操作。

        当我们完成一个物体的外轮廓标注之后，系统就会提醒我们接下来需要标注当前物体的关键点信息。点击 **开始关键点注释** 来开始关键点标注。

        .. image:: images/kp_notice.png
            :width: 800
            :align: center

        在物体上点击关键点的位置，随后在右侧的列表中选择对应的关键点名称，然后点击 **保存** 完成一个关键点的标注。

        .. image:: images/kp_label1.png
            :width: 800
            :align: center
        
        重复上述操作，直到将当前物体的所有关键点标注完成。全部关键点标注完成之后，点击 **完成** 即可完成一个物体的全部标注项目。

        .. image:: images/kp_label2.png
            :width: 800
            :align: center

+ **添加数据到数据集**

    当您标注完您的全部图像后，您需要把标注完的数据添加到你的数据集中，用于训练一个模型。进入标注界面点击右上角 **添加已标注的图片到数据集** ，就可以将您标注完的
    数据添加到数据集中，用于训练。

    .. image:: images/add2dataset.png
        :width: 800
        :align: center


+ **创建数据集版本**
    完成标注并将标注后的图像添加到数据集之后，点击创建便可以生成数据集的新版本(可以把它想象成数据的版本控制)。

    .. image:: images/create_train_set.png
        :width: 800
        :align: center
    
    在创建训练数据集之前，我们需要先使用内置的数据集健康检查工具来确认我们的数据集中是否含有可能会导致训练失败的标注问题。点击 **运行健康检查** ，等待数据集报告生成完成。

    .. image:: images/healthcheck.png
        :width: 800
        :align: center

    同时如果您还可以选择预处理和数据增强等功能。
    这里为了方便并未添加预处理和数据增强(需要注意的是，并不是预处理和数据增强选择的越多，模型训练的效果越好，只有适合的没有一定的)，然后点击创建我们就完成了
    一个数据集版本的创建。        

    DaoAI World对于部分训练模型提供了默认的预处理方法，如下图所示：

    .. image:: images/default_preprocess.png
        :width: 800
        :align: center

    更多关于预处理及数据增强方法及效果的信息，请参阅 :ref:`数据集管理`。
    
训练
-----------------------

    在完成上述步骤后我们就来到了最后一步训练环节，DaoAI World提供了多种训练模式以满足不同的模型应用场景需求。在关键点检测项目中，DaoAI World提供了 **快速** ， **精确** ，和 **旋转精确** 三种训练模式。您可以根据您的需求选择不同的训练模式。

    .. image:: images/training_mod.png
        :width: 800
        :align: center

    选择后，点击生成并开始训练，即可将任务添加到训练队列中

    .. image:: images/training.png
        :width: 800
        :align: center
    
    训练开始后，您可以查看实时的模型训练。需要的训练时间也显示在图表上方。
    .. image:: images/training_2.png
        :width: 800
        :align: center

    在DaoAI World自动进行模型训练的过程中您也可以通过查看实时训练图表查看模型的训练情况。等待DaoAI World将模型训练好后最终就会如下图所示，您可以选择导出模型。导出的模型可以使用 `DaoAI Vision Pilot <http://docs.welinkirt.com/daoai-vision-system-user-manual/chinese-2.24.4.0/index.html>`_ 或者 `DaoAI InspecTRA <http://docs.welinkirt.com/daoai-inspectra-user-manual/chinese-2.24.3.0/index.html>`_ 或者 DaoAI World SDK 详情请见 :ref:`开发功能`

    .. image:: images/training_over.png
        :width: 800
        :align: center

    也可以点击左侧导航栏的部署/测试，手动检测模型结果精确度。

    .. image:: images/deploy_test.png
        :width: 800
        :align: center

.. |br| raw:: html

      <br>