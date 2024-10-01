数据集
==========

上传数据
-----------------

    在项目创建成功之后，可以通过 **选择文件** 或 **选择文件夹** 或 **从其他项目导入** 的方式上传图片数据。上传的图片数据将会被添加到项目中，以供后续的标注工作。

        .. image:: Images/upload_folder.png
            :width: 800
            :align: center

    数据集支持.jpeg、.jpg、.png 和 .bmp 格式的图片文件, 以及.json格式的标注文件。如果上传的数据中包含.json格式的标注文件，系统会自动读取标注文件中的标注信息，并将其显示在标注信息中。您可以在此查看被识别到的标注信息是否正确，用以确保图片的标注没有出现错误。
    
        .. image:: Images/label_recog.png
            :width: 800
            :align: center

        .. tip::
            **从其他项目导入** 的方式现在支持导入多边形标注，自动转化成边界框。


        .. warning:: 
            请注意，由于系统限制，图片大小不能超过50MB。且单次上传图片建议小于1000张，单次上传图片数量过多可能会导致上传失败。
            



标签管理
~~~~~~~~~~~~~~

    您可以在类别页面管理项目中的标签

            .. image:: Images/label.png
                :scale: 60%
                :align: center

    点击右上角的 ``修改类别`` 可以打开标签编辑窗口，您可以新增或删除标签。

            .. image:: Images/edit_label.png
                :scale: 80%
                :align: center

    您可以点击标签边的颜色，来更改标签在项目中的 标签，和外轮廓的显示颜色。

            .. image:: Images/change_label_color.png
                :scale: 80%
                :align: center


使用2D相机采集数据
~~~~~~~~~~~~~~~~~~~~~~~

    如果您的工作场景中已经有架设好的2D相机，您也可以使用DaoAI Preview工具来使用2D相机现场采集数据上传，或者实时拍照测试已训练好的模型。

    在上传页面，可以点击 DaoAI Preview 的立即下载，然后下载DaoAI Preview

            .. image:: Images/daoai_preview.png
                :scale: 60%
                :align: center

    下载完成后，解压文件，然后打开 DaoAI Preview.exe 

            .. image:: Images/daoai_preview2.png
                :scale: 70%
                :align: center

            .. image:: Images/daoai_preview3.png
                :scale: 60%
                :align: center

    然后您可以看到软件界面，此时您还无法找到相机，需要使用 文件夹内附带的 zadig.exe 软件更改相机的驱动。

    **使用Zadig更改相机驱动**

    打开zadig.exe, 首先点击 options, 勾选 List All Devices

            .. image:: Images/zadig.png
                :scale: 80%
                :align: center

    选择您的相机的设备，然后安装 WinUSB (v6.1.7600) 驱动即可

            .. image:: Images/zadig_2.png
                :scale: 60%
                :align: center

    成功更改驱动后，相机即可被DaoAI Preview发现并连接相机，点击刷新，设备列表会自动更新，选择您要连接的相机，并点击 Connect

            .. image:: Images/daoai_preview_ui.png
                :scale: 80%
                :align: center

    调整相机的焦距，曝光，增益等，点击采集，即可看到画面。

            .. image:: Images/daoai_preview_cap.png
                :scale: 60%
                :align: center

    启用远程采集，点击复制网址, 回到DaoAI World, 点击立即连接并复制网址到主机信息栏。

            .. image:: Images/daoai_preview_net.png
                :scale: 70%
                :align: center

            .. image:: Images/daoai_preview_connect.png
                :scale: 60%
                :align: center

    连接成功后，点击 ``通过相机拍摄图像`` 即可采集图像到上传列表。

            .. image:: Images/daoai_preview_capture.png
                :scale: 70%
                :align: center

    继续点击通过相机拍摄图像 来继续采集，当采集完成后，点击保存并继续，即可上传到项目中。

            .. image:: Images/daoai_preview_caped.png
                :scale: 80%
                :align: center

标注工具
------------

        在DaoAI World中，您可以使用标注工具对图像进行标注。按照训练项目费雷的不同，您可以使用不同的工具对图像进行标注。

        标注时，可以使用右下角的 **图像显示设置** ，调整图像的显示，让标注更方便。

        .. image:: Images/image_display_setting.png
                :scale: 100%
                :align: center

        .. image:: Images/image_display_setting_adjust.png
                :scale: 100%
                :align: center
        
        常用的标注工具有：

            .. image:: Images/annoTool0.png
                :scale: 100%
                :align: center

            #. **拖动工具**
                * 快捷键：空格
                * 平移图像或选择/重新定位注释。
                * 使用方法：单击并拖动图像。

            #. **多边形工具**
                * 快捷键：P
                * 自由绘制注释，以获得更精确的形状。
                * 使用方法：单击以添加点，多边形将按照被添加的点的顺序包围框选区域，点击第一个点封闭多边形以完成多边形添加，或点击完成，最后一个被添加的点会自动与第一个点相连，封闭多边形区域。

            #. **智能多边形工具**
                * 快捷键：S
                * 使用智能助手绘制多边形。点击对象的中心，然后继续点击以添加或减去区域。
                * 使用方法：将鼠标移动到对象的中心后，将生成一个淡蓝色的区域框选当前多边形区域，移动鼠标以更改框选区域。点击左键确定多边形区域，继续单击左键将更多区域划分进多边形区域，右键单击将部分区域从多边形中剔除，完成多边形区域框选后，点击保存完成多边形区域标注。

            #. **用已有模型辅助标注**
                * 快捷键：Ctrl+B
                * 使用训练过的模型对当前数据集进行标注（根据模型的完成度，辅助标注可能会存在标注错误, 标注后需要手动检查标注是否正确）

            #. **重复上一个**
                * 快捷键：Ctrl+Y
                * 应用上一个图像上的所有注释（对于视频帧很有用）。

            #. **撤销**
                * 快捷键：Ctrl+Z 
                * 撤销上一个操作。

            #. **重做**
                * 快捷键：Ctrl+Shift+Z 
                * 重做上一个操作
                
            #. **标记为空**
                * 快捷键：Ctrl+M 
                * 当前图像中没有可标注物体。空图像是没有感兴趣的对象的图像。未注释的图像正在等待人工标注。

            您也可以在标注界面的右下角找到 键盘图标 展开快捷键列表
                
                .. image:: Images/hot_key.png
                    :width: 80%
                    :align: center
        
        在 **异常检测** 中，除了以上标记工具，还提供了 **无损标记** 功能：
            
            **无损标记**
                * 将当前图片标注为无损图像，即当前图片中没有异常物体。
    
                .. image:: Images/annoTool7.png
                    :width: 400
                    :align: center

        在 **物体检测** 中，我们使用 **边界框工具** 替换了 **多边形工具**：

            **边界框工具**
                * 快捷键：B
                * 用于标注物体的边界框。
                * 使用方法：单击左键以选择边界框的一角，移动鼠标框选物体，再次单击左键完成物体框选。

                .. image:: Images/annoTool8.png
                    :width: 400
                    :align: center
        
        在 **分类检测** 中，由于项目对整张图片进行分类，所以在 **分类检测** 中未提供标注工具，直接将标签分配给整张图片即可。
                
                .. image:: Images/class_label.png
                    :scale: 100%
                    :align: center

        在 **智能标注** 时，可以选择 简单，平滑，和复制三种智能标注风格, 并且标注时这个页面可以被鼠标拖动，如果标注时被这个窗口挡住，则可以通过移动该窗口来避免阻挡。

                .. image:: Images/sam_options.png
                    :scale: 100%
                    :align: center
                
            - **简单** ：边缘尽可能使用更少的点来绘制简单的掩膜。

                .. image:: Images/simple.png
                    :scale: 60%
                    :align: center
                
            - **平滑** ：使用合适的点数来绘制边缘，使其尽可能的贴合物体表面并边缘平滑
                .. image:: Images/normal.png
                    :scale: 60%
                    :align: center
            
            - **复杂** ：使用尽可能多的点数来绘制边缘，使其最大程度贴合物体的表面并保留任何边缘中的细节
                .. image:: Images/complex.png
                    :scale: 60%
                    :align: center                    

        各个模型的具体标注方法可参考 :ref:`模型`。

高分辨率图像切分标注
~~~~~~~~~~~~~~~~~~~~~~~~

        对于高分辨率图片，智能标注工具有时无法准确贴合物体的边缘。这是因为深度学习模型在推理时通常需要固定的图像分辨率，图像分辨率越大，处理时的压缩越多，从而影响标注的精度。

        通过高分辨率图像切分标注，图像被切分为更小的区域，从而降低每个区域的分辨率。这使得智能标注工具能够更精确地标注目标物体，提升标注效果。

        分割标注会将图片先分割，然后在每个小图中标注目标物体，标注完成后，算法会将标注数据整合到原始图片上，实现更精准的大分辨率图片中的物体标注。

            .. image:: Images/cut_label.png
                :scale: 60%
                :align: center

        标注时，可以在右上角选择切分图像的大小以及切分重叠。当物体切分切割到物体部分的时候，就可以通过切分重叠来使相邻的两张图片的其中一张可以完整的看到该物体，然后另一张看不到完整物体的图片就不做标注。

        左下角可以看到大图分割后的缩略图，并且当前标注的区域有高亮显示，标注完一个区域后，只需要点击下一个区域就可以继续标注。全部标注完成后，就可以正常切换下一张图片继续标注了。

            .. image:: Images/cut_label2.png
                :scale: 60%
                :align: center




标注数据管理批次
--------------------

        当您将一批数据上传到DaoAI World上后，您上传的这批数据会被整合成一个批次。他们是一组图像，您可以在DaoAI World的标注界面中查看，您可以在 **标注中** 
        列表对每个批次单独进行标注和管理，同时把已经标注完成的批次添加到数据集

        .. image:: Images/manage_unanno.png
            :width: 800
            :align: center
        
        **批次管理**

        * 上传未标注的图像后，它们将被分配到"标注中列表中。
        * 您可以对一批数据进行多次标注，标注完成后，可以点击 ``将已标注的图像添加到数据集`` 把标注好的数据添加到数据集中，批次中任何剩余的未标注的数据将被划分为一个新的批次。

        **删除批次**

        .. warning::
            请注意，删除是永久性且不可逆转的。您需要确保此操作不会对您接下来的项目造成影响才可以进行删除操作。
        
        您可以通过单击批次右上角的三个点并点击“删除数据集”来删除批次。

        .. image:: Images/delete.png
            :width: 600
            :align: center


        .. warning::
            再次提示，删除是永久性且不可逆转的。您需要确保此操作不会对您接下来的项目造成影响才可以进行删除操作。


数据集管理
----------------

搜索数据
~~~~~~~~~~~

    您可以通过上方的搜索栏按名称搜索图片

            .. image:: Images/search.png
                :width: 800
                :align: center

    也可以按照训练，验证，测试集来过滤搜索的结果

            .. image:: Images/search_filter.png
                :width: 800
                :align: center


更改图片为训练集，验证集，测试集
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    图片的左下方会有图标显示该图片是在训练集，验证集，或者测试集。

    您可以选中任何图片，并选择将它们添加到训练集，验证集，或者测试集。

            .. image:: Images/change_set.png
                :width: 800
                :align: center

删除数据
~~~~~~~~~~~~~~~~~~

        在DaoAI World中，您可以删除已经上传的图片。删除图片是一个不可逆的操作，删除后的图片将无法恢复。请谨慎操作。

        您可以在数据集中进行单张或多张图片删除的操作。

        在数据集页面中，将鼠标移至想要删除的图片上，点击图片右上角以选择图片：

            .. image:: Images/deletepng0.png
                :width: 800
                :align: center
        
        然后点击右上角的按钮，在下拉菜单中选择“从项目中移除”，即可移除选择的图片：

            .. image:: Images/deletepng1.png
                :width: 800
                :align: center
        
        如果您想要删除多张图片，可以通过同时选择多张图片，然后同时删除它们。


导出数据集
---------------

        从 DaoAI World 中导出数据。

        您可以随时从 DaoAI World 导出数据。您可以使用 DaoAI World Web 界面导出数据。

        要导出数据集，首先要点击DaoAI World项目界面的侧栏中的 **数据集**，在数据集界面中点击 **导出数据集** 按钮导出数据。

        .. image:: Images/export_data.png
            :width: 800
            :align: center
        
        在"导出数据集"界面 可以选择将数据下载为文件.zip或使用curl从命令行中下载。

        .. image:: Images/export_data_select.png
            :width: 600
            :align: center      