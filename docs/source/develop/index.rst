Development Features
====================

This chapter provides a detailed overview of the usage and functionalities of DWSDK.

DWSDK offers multiple types of SDKs for developers, allowing flexible use based on specific needs:

1. :ref:`Local SDK`
    The Local SDK provides interfaces for `C++ <./cppsetup.html>`_, `C# <./cssetup.html>`_, and `Python <./pysetup.html>`_ that run directly in the local environment. It uses DWSDK's core libraries, enabling developers to integrate inference capabilities deeply with local projects.

    Advantages:
        - **Performance Advantage**: Relies on local hardware for inference, leveraging CPU or GPU performance, ideal for applications requiring real-time responses.
        - **Low Latency**: Inference processes are completed entirely locally, without network transmission.
        - **Data Privacy**: No need to upload data to the cloud or servers, suitable for applications with high privacy requirements.

    Use Cases:
        - **Real-Time Scenarios**: For example, in production lines requiring rapid responses.
        - **Offline Scenarios**: Environments without network support, such as edge computing tasks in industrial or embedded devices.
        - **Autonomy and Control**: Projects with strict control requirements over hardware, operating systems, and libraries, such as high-security industrial applications.

2. :ref:`C++ Local HTTP Inference`
    `C++ Local HTTP Inference <./inference_service.html>`_ is a specialized implementation that runs within the Local SDK environment and interacts with DWSDK via HTTP API to perform inference.

    Advantages:
        - **Decoupling from Third-Party Dependencies**: Local HTTP inference uses HTTP API calls to interact with DWSDK, avoiding direct conflicts with third-party dependencies (e.g., OpenCV). Developers can configure project dependencies freely without affecting other modules.
        - **Lightweight Deployment**: Enables project independence, requiring only the service to run inference.
        - **Improved Maintainability**: The HTTP API-based interaction simplifies service upgrades and maintenance, allowing developers to quickly adapt to different SDK versions.

    Use Cases:
        - Scenarios requiring high compatibility with dependencies or minimizing conflicts with DWSDK.
        - Ideal for isolating, independently deploying, and managing DWSDK in various environments.

3. :ref:`DaoAI World Cloud Inference Service`
    `Python Cloud Inference Service <./dw_http_inference_client.html>`_ accesses DaoAI World cloud servers through HTTP API calls to perform inference. Images are uploaded to the cloud, where the deployed models handle the inference.

    Advantages:
        - **No Local Hardware Requirements**: Inference relies entirely on cloud resources, eliminating the need for high-performance local hardware.
        - **Rapid Deployment**: Avoids complex local environment setups, offering ready-to-use inference capabilities.
        - **Dynamic Model Updates**: Cloud models can be updated at any time, allowing local applications to use the latest versions without redeployment.
        - **High Scalability**: Supports multi-user concurrent scenarios with dynamic server resource scaling for larger applications.

    Use Cases:
        - Lightweight clients or resource-constrained devices (e.g., embedded systems).
        - Large-scale inference tasks requiring distributed inference using cloud computing resources.

4. :ref:`DaoAI Unsupervised Defect Detection SDK`
   The DaoAI Unsupervised Defect Detection SDK is specially designed for .8 versions Unsupervised Defect Segmentation models，which is standard sdk will not support in .8 version.
   The DaoAI Unsupervised Defect Detection SDK is only available in **C++** 
   It offers the following features:

   - **Load Pretrained Models for Inference**:
       - Supports pixel-level and image-level inference.

   - **Train Custom Models**:
       - Uses user-provided image data (primarily good samples) to train a dedicated defect detection model.

   Advantages:
       - **High Adaptability**: Supports various data formats and hardware environments.
       - **Powerful Anomaly Detection**: Efficiently captures abnormal patterns using unsupervised learning methods.
       - **Easy Integration**: Seamlessly integrates with local projects, offering flexible interface support.

   Use Cases:
       - Industrial production lines requiring defect detection for specific products.
       - R&D projects needing flexible model parameter adjustments to adapt to diverse data.
       - Scenarios with high privacy requirements, avoiding the need to upload data to the cloud.

With these three types of SDKs and services, developers can choose the most suitable tools and methods to achieve efficient and flexible inference application development.

.. toctree::
    :maxdepth: 1
    :hidden:
    
    local_sdk
    inference_service
    dw_http_inference_client
    unsupervised_sdk
    redistribution
    appendix
    faq
