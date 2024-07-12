异常检测
==========================================

**异常检测** 可以识别物体是否处在异常状态，如有破损，变形等。

    .. image:: Images/ano.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/daoaiworld_ano.mp4" type="video/mp4">
            </video>
        </div>

|

完成模型标注后，可以参考 :ref:`训练` 章节下的视频，创建数据集版本并训练部署。

模型选择场景
------------------------------------------

**异常检测** 作用于单一物体，即数据集中只包含一种物体，并且物体的位置需要保持相对固定，该物体分为正常和异常两种状态。

**异常检测** 会学习并判断物体是否处在异常状态，并使用多边形标注出物体的异常区域。

标注方法
----------------

如果有已经训练过的模型，可以使用辅助标注工具，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

如果物体没有缺陷，请标注为正常

    .. image:: Images/anoAnno2.png
        :scale: 100%

如果物体存在缺陷，请标注为异常。使用多边形工具，或者智能多边形，标注出异常区域的外轮廓。

    .. image:: Images/anoAnno1.png
        :scale: 100%

注意事项
------------

1. **异常检测** 项目中只能存在一个标签，用以标注物体出现异常的区域。

2. **标注正常** 的物体不应含有损害区域标签，否则可能会导致训练结果不理想，或训练失败。

3. 一个物体可以同时有多个损坏标签用来标注多个损坏区域。

.. note::

    1. 异常检测 项目在训练时，需要确保分配的训练集中所包含的图片数量小于等于该数据集中全部无损图片的数量，否则可能会导致训练失败。 同时，数据集中所包含的损坏图片的数量过少也会导致训练结果不理想。
    
    2. 异常检测 于其他项目不同，默认不添加任何数据增强选项。

练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 anomaly_detection.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。