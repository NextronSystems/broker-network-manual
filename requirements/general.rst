.. index:: General Understanding

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
For details, see :ref:`administration/agents:agent installer`.