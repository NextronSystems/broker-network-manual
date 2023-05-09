.. index:: Network Requirements

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
    The Lobby should not be exposed to the open internet if possible.
    You should deploy your Lobby in your internal network, so most of
    your agents can perform the necessary steps to be allowed into the
    broker network. The Lobby is the first point of contact for new agents
    and this onboarding phase is happening once during the initial
    communication, so that the agents can get their unique key material
    for the secure channel  with the broker endpoints. For more information
    on how the Lobby operates, see the chapter :ref:`administration/lobby_usage:using the lobby`.

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
make sure to configure the necessary A-Records before setting up an external facing Broker.