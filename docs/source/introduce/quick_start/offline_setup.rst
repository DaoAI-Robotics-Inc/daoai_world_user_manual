DaoAI World Offline Server Setup Instructions
==================================================================

This section is designed to help users access the DaoAI World offline version via a local area network (LAN). By modifying the client's ``hosts`` file, you can directly access DaoAI World services through a browser.

Preparations
------------------

Before starting, ensure that:

- The DaoAI World server is up and connected to the LAN.

- The client device is on the same LAN subnet as the server.

- You have permission to modify the hosts file on the client device.

Server Setup
--------------

The DaoAI World offline server comes with dual network ports and supports two access methods:

1. **DHCP Auto-Assigned IP Address** : The server can obtain an IP address automatically via the network. You can consult with the network administrator to find out the server's current IP address and use it to access the server.

2. **Static IP Address**: The server also has a fixed IP address of 192.168.1.10. You can directly use this IP for access.

Client Setup
--------------

To access DaoAI World services via a domain name on the client device, you need to modify the local ``hosts`` file to map a specific domain name to the server's IP address.

.. warning::

    DHCP Access: You need to know the IP address assigned to the DaoAI World server on the LAN. |br|
    Example Setup: The following instructions use a DaoAI World offline server with IP ``192.168.10.61`` as an example. |br|
    Static IP Access: You can replace the IP address in the following instructions with ``192.168.1.10`` |br|

Modifying the ``hosts`` file (Windows)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open Notepad with administrator privileges:
    - Click the Start menu and search for "Notepad"
    - Right-click "Notepad" and select **"Run as administrator"**

2. Open the ``hosts`` file:
    - In Notepad, click **"File"** -> **"Open"** .
    - Navigate to ``C:\Windows\System32\drivers\etc\hosts`` .
    - Ensure the file type is set to **"All Files"** , then select the ``hosts`` file and open it.

3. Add domain mapping:
    - At the end of the file, add the following lines:

    .. code-block::

        1 192.168.10.61 offline.we.link
        2 192.168.10.61 dw.offline.we.link
        3 192.168.10.61 api.offline.we.link
        4 192.168.10.61 s3.offline.we.link
        5 192.168.10.61 admin.offline.we.link

4. Save and close the file:
    - Click **"File"** -> **"Save"**, then close Notepad.

Modifying the ``hosts`` file (Linux/MacOS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the terminal:
    - On MacOS, use "Spotlight" to search for "Terminal."
    - On Linux, press ``Ctrl+Alt+T`` to open the terminal.

2. Edit the ``hosts`` file:
    - Enter the following command to open the ``hosts`` file using the ``nano`` editor:

    .. code-block::

        sudo nano /etc/hosts

    - Enter the system password if prompted.

3. Add domain mapping:
    - At the end of the file, add the following content:

    .. code-block::

        192.168.10.61 offline.we.link
        192.168.10.61 dw.offline.we.link
        192.168.10.61 api.offline.we.link
        192.168.10.61 s3.offline.we.link
        192.168.10.61 admin.offline.we.link

4. Save and close the file:
    - Press ``Ctrl+X`` to close the nano editor.
    - Press ``Y`` to confirm saving the changes.
    - Press ``Enter`` to return to the terminal.


Accessing DaoAI World Services
---------------------------------------

After completing the configuration, you can access DaoAI World services by entering the following URL in your browser:

    - `<http://dw.offline.we.link>`_

Troubleshooting
----------------------


If you cannot access DaoAI World services, check the following:

1. **Network Connection** : Ensure both the server and the client are connected to the same LAN.
2. **IP Address** : Ensure the server's IP address matches the one configured in the ``hosts`` file.
3. **Domain Resolution** : Use the ``ping`` command to check if the domain is correctly resolving to the server's IP address. For example:

.. code-block::

    ping dw.offline.we.link

4. **Cache Issues** : If domain resolution is incorrect, try clearing your browser cache or restarting the device.


If you continue to experience issues, please contact DaoAI technical support for assistance.

.. |br| raw:: html

      <br>