
Before You Begin
================

Agent to ASGARD Communication
-----------------------------

There are a few things to consider before you start with the installation of you Broker Network.
The communication between the ASGARD agent and the Broker Network is unidirectional.
The ASGARD agent polls ASGARD, or one of the Brokers if configured, in a given time frame
and looks for tasks to execute. There is no active triggering from ASGARD or the Broker(s)
to the ASGARD agent – we have designed it that way, because we believe that opening a port
on all connected endpoints should and can be avoided. 

The Broker Network acts as a gateway between ASGARD Agents and ASGARD itself. This allows
for more flexibility within your ASGARD environment, such as remote agents which are not 
using a VPN, or a dedicated Broker in your DMZ.

If an ASGARD Agent is configured to work with your Broker Network, it can still connect
directly to your ASGARD if the Broker can't be reached.

Overview of the Components
^^^^^^^^^^^^^^^^^^^^^^^^^^

There are three components which are needed for the Broker Network:

   * **Lobby** - New ASGARD Agents will get a certificate for a secure communication
     from the Lobby. An administrator can accept the agents or configure the auto-accept
     option. Certificates for agents can also be revoked here.
   * **Gatekeeper** - The Gatekeeper is used to communicate directly between all the
     components. Certificates and Revoke Lists get picked up from the Lobby and are
     being pushed to all Brokers.
   * **Broker** - Your Broker is the component which your ASGARD Agents directly
     communicate with. Once an ASGARD Agent received a valid certificate from the
     Lobby, communication is possible. You can have multiple Brokers configured.

.. figure:: ../images/broker_network_overview.png
   :alt: The Broker Network

Using a Proxy between ASGARD Agent and ASGARD
---------------------------------------------

ASGARD supports using a standard HTTP proxy for the entire Agent to ASGARD communication.
In order to use a proxy, the ASGARD agent must be repacked after installation.
For details, see :ref:`usage/administration:agent installer`.

Hardware Requirements
---------------------

You can find the hardware requirements for all of the components below.

ASGARD Broker Hardware
^^^^^^^^^^^^^^^^^^^^^^

The required hardware for your Broker depends on the setup you are choosing.

If you want to use only one Broker, you can use the hardware requirements from the table below.
If you want to use multiple Brokers, you can split the hardware requirements evenly among your Brokers.
This scenario might be useful for networks with multiple segments to keep a proper segmentation.

.. list-table::
   :header-rows: 1
   :widths: 35, 65

   * - Connected Endpoints
     - Combined Hardware Requirements
   * - up to 500
     - CPU Cores: 1
      
       System memory: 4 GB
       
       Hard Disk: 80 GB
   * - up to 10,000
     - CPU Cores: 4
      
       System memory: 6 GB
      
       Hard Disk: 200 GB
   * - up to 25,000
     - CPU Cores: 10

       System memory: 16 GB
      
       Hard Disk: 500 GB

Your Broker uses roughly 1 CPU Core for 2,500 agents. Generally we do recommend to use
the approach with multiple smaller Brokers instead of one big Broker.

Example: For an environment of up to 10,000 agents, you can use the following hardware
(per Broker; assuming all 10,000 agents communicate over the Broker Network):

  * 1 Broker
    
    - CPU Cores: 4
    - System Memory: 6 GB
    - Hard Disk: 200 GB
  * 2 Brokers
    
    - CPU Cores: 2
    - System Memory: 3 GB
    - Hard Disk: 100 GB
  * 4 Brokers
    
    - CPU Cores: 1
    - System Memory: 3 GB
    - Hard Disk: 80 GB

.. note:: 
  Try not to go lower than 80 GB of storage and 3 GB of system memory for
  your Broker, as this might influence system stability after a while.

ASGARD Gatekeeper Hardware
^^^^^^^^^^^^^^^^^^^^^^^^^^

The ASGARD Gatekeeper uses roughly the same amount of resources as
your `ASGARD Management Center <https://asgard-manual.nextron-systems.com/en/latest/usage/requirements.html#hardware-requirements>`_,
apart from the disk space. Please orientate yourself on the configuration
of your ASGARD. The recommendations are the following:

.. list-table::
   :header-rows: 1
   :widths: 30, 70

   * - Connected Endpoints
     - Minimum  Hardware Requirements
   * - up to 500
     - System memory: 4 GB
       
       Hard disk: 200 GB
       
       CPU Cores: 2
   * - up to 10,000
     - System memory: 8 GB
      
       Hard disk: 250 GB
       
       CPU Cores: 4
   * - up to 25,000
     - System memory: 16 GB
      
       Hard disk: 300 GB
       
       CPU Cores: 4

ASGARD Lobby Hardware
^^^^^^^^^^^^^^^^^^^^^

.. list-table::
   :header-rows: 1

   * - Hardware
     - Amount
   * - CPU Cores
     - 2
   * - System Memory
     - 4 GB
   * - Disk
     - 80 GB

Network Requirements
--------------------

The ASGARD components use the ports in the following chapters.
For a detailed and up to date list of our update and licensing
servers, please visit https://www.nextron-systems.com/hosts/.

ASGARD Agent
^^^^^^^^^^^^

.. list-table:: 
   :header-rows: 1
   :widths: 30, 20, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - Agent / Server communication
     - 443/tcp
     - ASGARD Agent
     - Broker / ASGARD
   * - Retrieve certificate
     - 443/tcp
     - ASGARD Agent
     - Lobby

.. note::
    The Lobby should not be exposed on the open internet. You can deploy
    your Lobby in your internal network and let all the agents pick up a
    certificate once they are being installed. The communication between
    Agents and the Lobby is happening once during the initial communication,
    so that the Agents can get their key material for the secure channel.

Gatekeeper
^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 30, 20, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - Statistics
 
       pull CA [2]_ and CRL [3]_
     - 12000/tcp
     - Gatekeeper
     - Lobby
   * - Statistics

       push CA [2]_ and CRL [3]_
     - 12000/tcp
     - Gatekeeper
     - Broker
   * - Create secure tunnel per client
     - 12001-1200x/tcp
 
       (x = CPU count of Broker)
     - Gatekeeper
     - Broker

.. note:: 
    Your Gatekeeper is getting the CA and revoked certificates from the Lobby.
    Those certificates are in return sent to the all Brokers.

.. [2]
   Root CA Certificate (CA)

.. [3]
   Certificate Revocation List

ASGARD
^^^^^^

.. list-table:: 
   :header-rows: 1
   :widths: 30, 20, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - Backend management of Gatekeeper, Broker and Lobby
 
       Agent communication
     - 12000/tcp
     - ASGARD
     - Gatekeeper

Management Workstation
^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: 
   :header-rows: 1
   :widths: 30, 20, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - CLI administration
     - 22/tcp
     - Workstation
     - Broker
   * - CLI administration
     - 22/tcp
     - Workstation
     - Gatekeeper
   * - Web administration
     - 9443/tcp
     - Workstation
     - Lobby
   * - CLI administration
     - 22/tcp
     - Workstation
     - Lobby

Internet
^^^^^^^^

The Broker Network components are configured to retrieve updates from the following remote system.

.. list-table:: 
   :header-rows: 1
   :widths: 25, 15, 25, 35

   * - Description
     - Port
     - Source
     - Destination
   * - Product and system updates
     - 443/tcp
     - Gatekeeper, Lobby, Broker
     - update3.nextron-systems.com
   * - NTP
     - 123/udp
     - Gatekeeper, Lobby, Broker
     - 0.debian.pool.ntp.org [4]_
   * - NTP
     - 123/udp
     - Gatekeeper, Lobby, Broker
     - 1.debian.pool.ntp.org [4]_
   * - NTP
     - 123/udp
     - Gatekeeper, Lobby, Broker
     - 2.debian.pool.ntp.org [4]_

.. [4]
  The NTP server configuration can be changed.

All proxy systems should be configured to allow access to these URLs without
TLS/SSL interception. (ASGARD uses client-side SSL certificates for authentication).
It is possible to configure a proxy server, username and password during the setup
process of the ASGARD platform. Only BASIC authentication is supported (no NTLM authentication support).

DNS
^^^

All the components need to have a resolvable FQDN.

Brokers facing the open internet need to be resolvable with a FQDN, so
make sure to configure the necessary A-Records before setting up an external facing Broker.


Verify the Downloaded ISO (Optional)
------------------------------------

You can do a quick hash check to verify that the download was not corrupted.
We recommend to verify the downloaded ISO's signature as this is the cryptographically sound method.

The hash and signature file are both part of the ZIP archive you download from our `portal server <https://portal.nextron-systems.com>`__.

Via Hash
^^^^^^^^

Extract the ZIP and check the sha256 hash:

On Linux

.. code-block:: console

    user@host:~$ sha256sum -c nextron-universal-installer.iso.sha256
    nextron-universal-installer.iso: OK

or in Windows command prompt

.. code-block:: doscon

    C:\Users\user\Desktop\asgard2-installer>type nextron-universal-installer.iso.sha256
    efccb4df0a95aa8e562d42707cb5409b866bd5ae8071c4f05eec6a10778f354b  nextron-universal-installer.iso
    C:\Users\user\Desktop\asgard2-installer>certutil -hashfile nextron-universal-installer.iso SHA256
    SHA256 hash of nextron-universal-installer.iso:
    efccb4df0a95aa8e562d42707cb5409b866bd5ae8071c4f05eec6a10778f354b
    CertUtil: -hashfile command completed successfully.  

or in Powershell

.. code-block:: ps1con

    PS C:\Users\user\Desktop\asgard2-installer>type .\nextron-universal-installer.iso.sha256
    efccb4df0a95aa8e562d42707cb5409b866bd5ae8071c4f05eec6a10778f354b  nextron-universal-installer.iso
    PS C:\Users\user\Desktop\asgard2-installer>Get-FileHash .\nextron-universal-installer.iso

    Algorithm       Hash                                                                   Path
    ---------       ----                                                                   ----
    SHA256          EFCCB4DF0A95AA8E562D42707CB5409B866BD5AE8071C4F05EEC6A10778F354B       C:\Users\user\Desktop\asgard2-installer\nextron-universal-installer.iso

Via Signature (Recommended)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Extract the ZIP, `download the public signature <https://www.nextron-systems.com/certificates-and-keys>`__ and verify the signed ISO:

On Linux

.. code-block:: console

    user@host:~$ wget https://www.nextron-systems.com/certs/codesign.pem
    user@host:~$ openssl dgst -sha256 -verify codesign.pem -signature nextron-universal-installer.iso.sig nextron-universal-installer.iso
    Verified OK

or in powershell

.. code-block:: ps1con

    PS C:\Users\user\Desktop\asgard2-installer>Invoke-WebRequest -Uri https://www.nextron-systems.com/certs/codesign.pem -OutFile codesign.pem
    PS C:\Users\user\Desktop\asgard2-installer>"C:\Program Files\OpenSSL-Win64\bin\openssl.exe" dgst -sha256 -verify codesign.pem -signature nextron-universal-installer.iso.sig nextron-universal-installer.iso
    Verified OK 

.. note::

    If ``openssl`` is not present on your system you can easily install it using winget: ``winget install openssl``.