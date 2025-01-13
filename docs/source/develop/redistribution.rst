SDK Redistribution
==================

This section explains how to redistribute the DaoAI World SDK to your customers. Depending on your use case, you can choose from the following two methods for SDK redistribution:

Method 1: Customers Install DW SDK Themselves
---------------------------------------------
In this method, customers manually install the DW SDK package and configure the runtime environment. The steps are as follows:

1. Provide customers with the **DW SDK installation package** and installation guide.  
   - You can obtain the latest download link from :ref:`Installation`.

2. Customers complete the installation following the instructions:  
   - The installation path can be the default or customized based on their needs.  
   - After installation, ensure the SDK's runtime library path is added to the system's `PATH` environment variable.

3. Provide your application files (e.g., `exe` files). Once customers install the DW SDK, they can run your application.

**Use Cases**:
- Customers can easily access and install the DW SDK.
- Customers need to use the full functionality of the DW SDK, such as developing new features or testing other models.

Method 2: Bundle DW SDK Runtime Files
-------------------------------------
In this method, you bundle the essential runtime files of the DW SDK with your application, allowing customers to run the program directly without needing to install the SDK. The steps are as follows:

1. Ensure the DW SDK is installed locally and functioning properly.

2. Copy the following files and folders from the DW SDK installation directory to your application directory:  
   - **`bin` folder**: Contains the core dynamic link libraries (DLL files) of the DW SDK.  
   - **`3rdparty` folder**: Contains the dynamic link libraries (DLL files) of third-party dependencies.

3. Place these files in the same directory as your executable file (`exe` file) to ensure the dynamic link libraries can be loaded correctly.

4. Provide the bundled directory to your customers. They can run your application directly without installing the SDK.

**Use Cases**:
- Customers do not need the full DW SDK and only use the features integrated into your application.
- Simplifies deployment and is suitable for distributing standalone applications.

Considerations
--------------
- **Compatibility Testing**: Test the bundled application on different operating systems and environments to ensure it runs without issues.  
- **File Integrity**: Avoid omitting any necessary dynamic link library files to prevent runtime errors.

Summary
-------
Choose the method that best suits your business needs to redistribute the DW SDK to your customers. If you're unsure which method to use, refer to the use cases above or contact technical support for assistance.
