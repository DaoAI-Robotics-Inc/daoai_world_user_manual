常见问题 
===========

.. contents::
    :local:

图片上传后，在标注页面打开是全黑的图片
--------------------------------------------------

    .. image:: images/invalid_images.png
        :align: center

    上传图片时，DaoAI World 会对上传的数据进行检查和验证，部分图片可能会出现上传失败(如上图)。这是因为图片可能出现了损坏，建议重新采集图片。或者使用以下的方式尝试：

方法1：尝试修复损坏数据
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    使用 **XnView MP** 图片工具， `下载连接 <https://daoairoboticsinc-my.sharepoint.com/:u:/g/personal/nrd_daoai_com/EWlgNZq_aBNFuomgwXGDx_QBzG2SBuqYFRd724qvd1TJXw?e=33ebHp>`_ 

    解压文件后，找到 **XnView MP** 图片工具的执行程序，双击运行。

    .. image:: images/xnviewmp_exe.png
        :align: center
        :scale: 50%

    左上角 `文件` ， `打开` 选取损坏的图片。

    .. image:: images/xnviewmp_open_file.png
        :align: center
        :scale: 50%
    
    左上角 `文件` ， `另存为` 保存图片到另外的路径下。

    .. image:: images/xnviewmp_save_as.png
        :align: center
        :scale: 50%

    将保存好的图片重新上传。

    .. image:: images/xnviewmp_upload_success.png
        :align: center
        :scale: 50%

    
部署后，物体在边缘时 模型推理失败
--------------------------------------------------
    
    当你发现物体在边缘时，模型无法匹配，报错时。

    .. image:: images/roi_fail.png
        :align: center
        :scale: 50%


    那么请检查以下模型训练时的预处理步骤是否有ROI设置，并且物体出现在了ROI区域以外

    .. image:: images/roi_fail_setting.png
        :align: center
        :scale: 70%

    
    由于感兴趣的区域（ROI）预处理会裁剪掉周围的背景区域，只保留中间的感兴趣的区域; 那么当物体出现在ROI范围之外时，会被当做背景的区域被裁剪掉，也就不会有预测结果了。

    如果您遇到了这种问题，那么您可以考虑：

    1. 更新ROI区域，通过重新定义ROI的区域，正确的预留物体可能出现的区域，确保物体只会出现在ROI区域, 并重新训练模型
    2. 删除ROI预处理设置，并重新训练模型

    然后您就可以正常的预测在图像边缘的物体了

    .. image:: images/roi_fail_result.png
        :align: center
        :scale: 50%

模型推理失败 （图片分辨率不同）
--------------------------------------------------
    
    当您尝试模型推理时 报错无法推理时，请检查使用时的模型分辨率 是否和训练数据中的分辨率一致。

    通常来说，模型对分辨率的变化是有一定的适应能力的

    可是如果增加了ROI预处理步骤，ROI会根据原始图片的分辨率进行切割

    如果这时，使用了比训练图片分辨率小很多的图片进行推理，那么整张图片都会被ROI的部分切割, 就会导致图片推理失败。

    **例如** , 训练集中的图片宽8000像素，ROI预处理步骤切割了左右两边各1000像素的背景区域。当您使用一个1000分辨率或以下的图片进行推理时，如下图左上角，预处理步骤将会按照原图的背景进行切割，也就时会把1000点像素删掉，那么这样就会完全将推理的图片删除，导致模型推理失败。

    .. image:: images/res_fail.JPG
        :align: center
        :scale: 50%


    所以请您在使用时 用和训练集图片分辨率一致的图片进行测试和推理
