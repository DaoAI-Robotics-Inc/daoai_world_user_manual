Development
=================

This chapter provides a detailed introduction to the usage and features of DWSDK.

DWSDK offers various types of SDKs, allowing developers to choose the most suitable option based on their specific requirements. The available options are as follows:

1. Local SDK
   The local SDK provides interfaces in multiple programming languages, including `C++ <./cppsetup.html>`_, `C# <./cssetup.html>`_, and `Python <./pysetup.html>`_, which run directly in the local environment. It leverages the core libraries of DWSDK, enabling developers to deeply integrate inference capabilities into their local projects.

   **Advantages:**
       - **Performance Benefits:** Utilizes local hardware to perform inference, taking full advantage of CPU or GPU capabilities, making it ideal for applications requiring real-time responses.
       - **Low Latency:** The entire inference process is executed locally, eliminating the need for network transmission.
       - **Data Privacy:** No data is uploaded to the cloud or server, suitable for applications with high privacy requirements.

   **Use Cases:**
       - **High-Responsiveness Environments:** Such as production lines requiring quick responses.
       - **Offline Scenarios:** Situations without network support, such as edge computing tasks in industrial or embedded devices.
       - **Self-Controlled Systems:** Projects with strict control over hardware, operating systems, and dependencies, such as high-security industrial applications.

2. C++ Local Inference Service
   The `C++ Local Inference Service <./inference_service.html>`_ is a specialized implementation that runs in the local environment, interacting with DWSDK via HTTP API to perform inference.

   **Advantages:**
       - **Decoupling of Third-Party Dependencies:** The local inference service uses HTTP API calls to interact with DWSDK, avoiding potential conflicts between project dependencies (e.g., OpenCV) and DWSDK.
       - **Lightweight Deployment:** Enables project independence, requiring only the service to run to achieve inference functionality.
       - **Improved Maintainability:** The HTTP API invocation model simplifies service upgrades and maintenance, allowing developers to quickly adapt to different versions of the SDK.

   **Use Cases:**
       - Projects requiring high compatibility with dependencies or minimizing dependency conflicts with DWSDK.
       - Scenarios requiring isolation, independent deployment, and management of DWSDK.

3. C++ HTTP Inference Service
   The `C++ HTTP Inference Service <./dw_http_inference_client.html>`_ uses HTTP API to access DaoAI World cloud servers, enabling inference by uploading images to the cloud and leveraging models deployed on the server.

   **Advantages:**
       - **No Local Hardware Requirements:** Inference relies entirely on cloud resources, eliminating the need for high-performance hardware on local devices.
       - **Rapid Deployment:** Provides ready-to-use inference capabilities without the need for complex local environment configuration.
       - **Dynamic Model Updates:** Cloud-based models can be updated at any time, allowing local users to access the latest versions without redeployment.
       - **High Scalability:** Supports large-scale applications through server resource scaling, making it suitable for multi-user concurrent scenarios.

   **Use Cases:**
       - Lightweight clients or resource-constrained devices (e.g., embedded systems).
       - Large-scale inference tasks utilizing cloud computing resources for distributed inference.

By combining these three SDKs and services, developers can select the most appropriate tools and methods to achieve efficient and flexible inference application development.

.. toctree::
    :maxdepth: 1 
    :hidden:

    install
    license
    cppsetup
    cssetup
    pysetup
    inference_service
    dw_http_inference_client
    appendix
    faq
