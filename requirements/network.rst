.. index:: Network Requirements

Network Requirements
--------------------

The ASGARD components use the ports in the following chapters.
For a detailed and up to date list of our update and licensing
servers, please visit https://www.nextron-systems.com/resources/hosts/.

ASGARD Agent
^^^^^^^^^^^^

.. list-table:: 
   :header-rows: 1
   :widths: 35, 15, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - Agent to Server communication
     - 443/tcp
     - ASGARD Agent
     - Broker / ASGARD
   * - Retrieve certificate
     - 443/tcp
     - ASGARD Agent
     - Lobby

.. warning::
  Your agents will always try to contact your ASGARD directly, and if this fails,
  they will try the Lobby or Brokers. If you are deploying agents with a
  broker network configuration in your internal network, and they **can** contact
  the ASGARD directly, they will not be able to get a valid certificate from
  your Lobby. This is not an issue if your Lobby is exposed to the internet and
  your agents will be able to request a certificate once they are connecting from
  the open internet.
  
  If your Lobby is not exposed to the internet, the agents **must not** be able to
  contact your ASGARD directly, but rather your Lobby. To do this, you have to
  ensure your agents will not be able to communicate directly with your ASGARD,
  but only directly with the Lobby and a Broker (e.g. your ASGARD sits behind an
  internal Broker and can not be reached directly).
  
  It is important to remember that your agents need a valid certificate from your
  Lobby, otherwise the Broker connection can not be established. This "onboarding
  phase" is happening once during the initial communication, so that your agents
  can get their unique key material for the secure channel within the broker
  network. For more information on how the Lobby operates, see the chapter
  :ref:`administration/lobby_usage:using the lobby`.

The following priorities of servers your agents try to connect to are in place:

.. list-table:: 
   :header-rows: 1
   :widths: 30, 20, 40

   * - Server
     - Priority
     - Info
   * - ASGARD
     - 1
     - Always highest priority
   * - Lobby
     - 2
     - If agent has no Broker Certificate
   * - Broker
     - 3
     - If agent has Broker Certificate

Gatekeeper
^^^^^^^^^^

.. list-table::
   :header-rows: 1
   :widths: 30, 20, 25, 25

   * - Description
     - Port
     - Source
     - Destination
   * - \- Statistics

       \- pull CA [2]_ and CRL [3]_
     - 12000/tcp
     - Gatekeeper
     - Lobby
   * - \- Statistics

       \- push CA [2]_ and CRL [3]_
        
     - 12000/tcp
     - Gatekeeper
     - Broker
   * - Create secure tunnel per client
     - 12001-1200x/tcp
 
       (x = CPU count of Broker)
     - Gatekeeper
     - Broker

.. note:: 
    Your Gatekeeper is receiving the root CA certificate, client certificates
    and CRL from the Lobby. Those are then being transmitted to the all Brokers
    via the Gatekeeper, to keep an up to date state of allowed and revoked agents.

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
   * - \- Backend management of Gatekeeper, Broker and Lobby
 
       \- Agent communication
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
   * - CLI administration
     - 22/tcp
     - Workstation
     - Lobby
   * - Web administration
     - 9443/tcp
     - Workstation
     - Lobby

Internet
^^^^^^^^

The Broker Network components are configured to retrieve updates from the following remote systems.

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

Brokers facing the open internet need to be resolvable with a public FQDN and IP Address, so
make sure to configure the necessary A-Records before setting up an external facing Broker
and/or Lobby.