分类检测
==================================

**分类检测** 应用于对不同场景的分类识别。需要注意的时，它并非用于对同一场景内的不同物体的区分。

    .. image:: Images/class.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/daoaiworld_class.mp4" type="video/mp4">
            </video>
        </div>

|

完成模型标注后，可以参考 :ref:`训练` 章节下的视频，创建数据集版本并训练部署。

模型选择情景
----------------------------------

**分类检测** 可以用于识别区分不同的场景。也可以用于识别区分不同的物体。但是，当使用 **分类检测** 用于物体区分时，需要保证场景中只有被区分物体。
**分类检测** 也可以用于同一物体的不同状态的区分。如，物体是否有损坏，或是否处在不同状态。同理我们需要确保场景中只有被区分物体。

标注方法
-------------

如果有已经训练过的模型，可以使用辅助标注工具，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

选择一个类，然后完成标注，一个图片只能拥有一个类别。

    .. image:: Images/classAnno0.png
        :scale: 100%

注意事项
--------------

1. **分类检测** 的标注目标为整张图片，而非图片中的某一单一物体。

2. **分类检测** 的数据集中，每张图片应只包含目标物体，以减少其他物体的影响。

3. 可以通过预处理中划定感兴趣区域来减少图片中的其他物体对模型的影响。

练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 image_classification.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。