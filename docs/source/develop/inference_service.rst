本地HTTP推理
--------------------------------------------

本地HTTP推理可以让您通过本地SDK，使用HTTP请求来执行模型的推理并获取结果。

    .. image:: images/inf_service.png
        :scale: 100%

在 DW SDK 安装目录下，您可以找到 inference_service.exe 以及 inference_service_gui.exe.

其中双击运行inference_service.exe 即可在后台启动推理服务，您可以在右下角任务图标中找到。而 inference_service_gui.exe 则会打开该服务的图形操作界面 （需要首先运行 inference_service.exe ）。

使用图形界面
~~~~~~~~~~~~~~~~

您可以通过图形界面注册和移除您的模型。

    .. image:: images/inf_gui.png
        :scale: 100%

点击 ``Add Model`` 来注册一个模型， 输入模型名称，选择模型的路径，然后选择模型的类型。点击 OK 即可注册模型。

删除模型时，选中一个模型，点击 ``Delete Model`` 即可删除模型。

使用HTTP请求
~~~~~~~~~~~~~~~~

您可以通过HTTP请求 发送至 localhost:5000 来管理您的模型，以及执行模型的推理并获取结果。

总共有4个请求可以使用，这里使用 Postman 来演示。

1. 注册模型：

    **Post** localhost:5000/register

    .. image:: images/inf_register.png
        :scale: 100%

    该命令可以讲本地的模型注册到推理服务里，可供之后的使用

2. 列出模型：

    **GET** localhost:5000/list

    .. image:: images/inf_list.png
        :scale: 100%

    该命令可以列出已经注册的模型

3. 模型推理：

    **Post** localhost:5000/inference

    .. image:: images/inf_inf.png
        :scale: 100%

    该命令指定一个模型，和一个本地图片路径，并获取该模型于指定图片的推理结果。

4. 模型删除：

    **DELETE** localhost:5000/delete

    .. image:: images/inf_del.png
        :scale: 100%

    该命令会讲指定的模型从推理服务中移除 (并不会删除本地模型文件)

示例项目
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Inference Client 通过和 Inference Service 交互， 极简化了环境配置的依赖。

比起 Windows SDK 需要使用许多的dll依赖， Inference Client 只需要1个。避免了在项目想要引入其它依赖时（如 Opencv）的dll版本冲突。

Inference Client 提供了与 Windows SDK 相似的接口。

详情请参考:

:ref:`C++ Inference Client 示例项目`
:ref:`C# Inference Client 示例项目`

您也可以查看我们的 GitHub repo，其中包含 C++、C# 和 Python 的示例项目，方便用户快速上手和参考。

链接： `DaoAI World SDK Desktop Demo <https://github.com/DaoAI-Robotics-Inc/DaoAI-World-SDK-Desktop-Demo>`_

SDK 接口文档
~~~~~~~~~~~~~~

`Inference Client C++接口文档 <../_static/doc_C_client/index.html>`_
`Inference Client C#接口文档 <../_static/doc_Cs_client/index.html>`_