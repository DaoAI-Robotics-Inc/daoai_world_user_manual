目标检测
===============================

物体检测可以用来识别场景中物体的种类时数量。不同于实例分割模型，物体检测不会准确识别物体的边框和精确位置。

    .. image:: Images/obj.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="https://docs.daoai.ca/static/videos/daoaiworld_obj.mp4" type="video/mp4">
            </video>
        </div>

|



模型选择情景
----------------------------------

**物体检测** 适用于判断某物体是否出现在场景中，或者判断某物体出现的次数。 

**物体检测** 可以用于识别场景中某一个或多个物体出现的次数和大概位置。物体识别可以同时识别多种物体。 但是物体识别不会返回物体的精确位置。 如果需要精确位置，可以使用 **实例分割** 模型，或者 **关键点检测** 模型。

标注方法
----------

如果有已经训练过的模型，可以使用辅助标注工具，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

使用矩形标注工具，或者智能标注，对物体进行边界框标注。

    .. image:: Images/objAnno1.png
        :scale: 100%

重复标注场景内所有的物体。如果场景内没有物体，请标注为空。

注意事项
--------------

1. **物体检测** 模型仅支持方形标注，因此，在物体检测模型标注中，标注区域可以轻微重合，但不可完全重合。

2. **物体检测** 模型标注中，标注区域不可超出图片边界。

3. 于其他标注模型类似，在标注时，应当避免标注被大面积覆盖的物体，选择最顶层或最明显的物体进行标注。


练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 object_detection.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。
