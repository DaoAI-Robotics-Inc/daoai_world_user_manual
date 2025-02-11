监督缺陷分割
==========================================

**监督缺陷分割** 可以识别物体是否处在异常(NG)状态，如有破损，变形等。与非监督缺陷分割检测不同，监督缺陷分割检测支持检测多种异常类型。

    .. image:: Images/sem.png
        :scale: 100%

    .. raw:: html

        <div style="position: relative; padding-bottom: 0.25%; height: 0; overflow: hidden; max-width: 80%; height: auto;">
            <video width="80%" height="auto" controls>
                <source src="http://docs.welinkirt.com/static/videos/dw_semseg-v6.mp4" type="video/mp4">
            </video>
        </div>

|

完成模型标注后，可以参考 :ref:`训练` 章节下的视频，创建数据集版本并训练部署。

模型选择场景
------------------------------------------

**监督缺陷分割** 作用于单一物体，即数据集中只包含一种物体，并且物体的位置需要保持相对固定，该物体分为正常和异常两种状态。

**监督缺陷分割** 的使用场景主要分为以下两点：

1. **分割特定区域**  判断特定区域的变化情况。例如消防通道的识别，如果有侵占，消防通道的分割区域就会发生变化
2. **分割异常区域**  不同与非监督缺陷检测模型，非监督缺陷检测只使用正常数据去训练识别异常，训练判断正常和异常。监督缺陷分割模型在异常图像数量较多的情况下，可以更细致的识别分割异常区域和种类。

标注方法
----------------

如果有已经训练过的模型，可以使用辅助标注工具，让深度学习模型来帮助您标注，然后您再检查以及纠正标注。
    .. image:: Images/suppor_anno.png
        :scale: 100%

如果物体没有缺陷，请标注为正常

    .. image:: Images/sem_anno0.png
        :scale: 80%

如果物体存在缺陷，请标注为异常。使用多边形工具，或者智能多边形，标注出异常区域的外轮廓，并选择缺陷类型。

    .. image:: Images/sem_anno1.png
        :scale: 80%

注意事项
------------

1. **标注正常** 的物体不应含有损害区域标签，否则可能会导致训练结果不理想，或训练失败。


练习
--------

从 `练习数据 <https://daoairoboticsinc-my.sharepoint.com/:f:/g/personal/nrd_daoai_com/EkNGNFG9C1ZCkejjwLZ4WOsBUQuhkn6apK4MSej2z1DfQA?e=ZOoc8v>`_ 中下载 supervised_data.zip

解压缩后您将得到11张图片和标注文件（.json）, 请您只上传图片到DaoAI World进行标注练习。之后可以一同上传图片和标注文件，对比结果。
