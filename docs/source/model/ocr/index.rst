OCR
==================================

**OCR** 应用于对图片中文字的提取。

    .. image:: Images/ocr.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="https://docs.daoai.ca/static/videos/daoaiworld_ocr.mp4" type="video/mp4">
            </video>
        </div>

|


模型选择情景
----------------------------------

**OCR** 可以识别并提取图片中的文字。

比如产品编号，日期，名称，等信息，都可以通过OCR模型，快速提取。

通常情况下，用户不需要自己训练，只用下载预训练的模型就可以在大部分场景中使用，如果有表现不佳的地方，再考虑增加训练。

OCR预训练模型可以在上方的模型体验里找到并下载。

    .. image:: Images/ocr_download.png
        :scale: 60%

标注方法
-------------

可以使用预训练OCR模型，来辅助标注，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

标注时，使用矩形标注工具，或者智能多边形工具，框选处标注的文字。然后输入对应的文字作为标签名称。
    .. image:: Images/ocr_anno.png
        :scale: 60%

重复标注场景内所有的物体。如果场景内没有物体，请标注为空。

练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 ocr.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。
