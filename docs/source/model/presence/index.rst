错漏装检测
==========================================

**错漏装检测** 可以根据与标准图像对比，检测图像中零件的安装情况。常用与零件的错装和漏装检测。

    .. image:: Images/pre.png
        :scale: 100%

    .. .. raw:: html

    ..     <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
    ..         <video width="80%" height="auto" controls>
    ..             <source src="http://docs.welinkirt.com/static/videos/dw_ano-v6.mp4" type="video/mp4">
    ..         </video>
    ..     </div>

|

完成模型标注后，可以参考 :ref:`训练` 章节下的视频，创建数据集版本并训练部署。

模型选择场景
------------------------------------------

**错漏装检测** 作用于判断某物体是否出现在场景中，或者判断某物体出现的次数。

**错漏装检测** 需要有标准图像作为学习对象，会学习并判断物体出现的次数。

标注方法
----------------

如果有已经训练过的模型，可以使用辅助标注工具，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

使用边界框标注工具(绿色)，或者智能标注(红色)，对物体进行边界框标注。

    .. image:: Images/pre_annotate.png
        :scale: 100%

错漏装检测模型需要有一个 **标准图像** 作为比较的基准，您需要在标注好标准图像后，对于布局进行设置，否则数据集无法完成标注。

    .. image:: Images/pre_golden_image.png
        :scale: 100%

点击 ``保存为标准图像`` 后，点击继续。

    .. image:: Images/pre_golden_continue.png
        :scale: 100%

.. warning::
    点击 ``保存为标准图像`` 会覆盖原先的标准图像，并重置布局。会导致标注失效并需要重新标注，请谨慎。

选择布局工具，框选出布局的位置。

    .. image:: Images/pre_layout.png
        :scale: 100%

布局可以是工件的整体范围，也可也是物体会出现的ROI范围。        

    .. image:: Images/pre_golden_board.png
        :scale: 100%

保存后您可以查看目前布局的标签，布局中的物体标签和数量。

    .. image:: Images/pre_golden_board_save.png
        :scale: 100%        

重复把所有需要识别的物件标注好。

    .. image:: Images/pre_annotate_all.png
        :scale: 100%

如果场景内没有物体，请标注为空。

    .. image:: Images/pos_null.png
        :scale: 100%


注意事项
------------

1. **错漏装检测模型** 标注时，应当先标注好标准图像，设置好布局。

2. **错漏装检测模型** 标注中，标注区域不可超出图片边界。

3. 于其他标注模型类似，在标注时，应当避免标注被大面积覆盖的物体，选择最顶层或最明显的物体进行标注。


练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 presence_checking.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。