.. index:: Components

Installing the Broker Network Components
----------------------------------------

After the base installation of your servers is completed, we can install the specific software for the components.

You can now choose the role you want to install (Broker, Gatekeeper or Lobby):

.. figure:: ../images/broker_nextronInstaller.png
   :alt: the nextronInstaller

You can install the three [1]_ servers in any order, as we will configure them once they are all up and running.

.. warning::
   The Broker Network needs a minimum version of 2.14.0 of the ASGARD
   Management Center. Please make sure you installed your Broker Network
   license in your ASGARD Management Center.
   If you still can't see the ``Broker Network`` tab in your
   ``Asset Management``, restart the ``asgard2`` service in ``Settings``
   > ``System`` > ``Services``.

.. [1]
   This number may vary. In this example we went with the minimum of one Broker, one Lobby and one Gatekeeper.

Gatekeeper Installation
^^^^^^^^^^^^^^^^^^^^^^^

To install the Gatekeeper, run the following command on your newly installed system:

.. code-block:: console
    
    nextron@gatekeeper:~$ sudo nextronInstaller -gatekeeper

.. figure:: ../images/setup_gatekeeper1.png
   :alt: Installing the Gatekeeper

After the installation is done, you will see the following message:

.. figure:: ../images/setup_gatekeeper2.png
   :alt: Installing the Gatekeeper

You can now check if the service was installed successfully. 

.. code-block:: console
   
   nextron@gatekeeper:~$ systemctl status asgard2-gatekeeper.service
   
You will see that the service is in a "**failed/exited**" state. This will
change once we configured our ASGARD with the Gatekeeper.

To configure your Gatekeeper in the ASGARD Management Center, we
will continue later in the chapter :ref:`administration/configuration:Gatekeeper Configuration`.

Lobby Installation
^^^^^^^^^^^^^^^^^^

To install the Lobby, run the following command on your newly installed system:

.. code-block:: console
   
   nextron@lobby:~$ sudo nextronInstaller -lobby

.. figure:: ../images/setup_lobby1.png
   :alt: Installing the Lobby

After a short while you will be prompted to enter a password for the
``admin`` user. This is the user for the web interface of the Lobby.

.. note:: 
   The password has to be:
      - A minimum of 12 characters long
      - Contain at least one upper- and lowercase letter, one digit and one special character

.. figure:: ../images/setup_lobby2.png
   :alt: Installing the Lobby

After the installation is finished, you will see the following message:

.. figure:: ../images/setup_lobby3.png
   :alt: Installing the Lobby

You can check the service to see if everything is up and running.

.. code-block:: console
   
   nextron@lobby:~$ systemctl status asgard-lobby.service

.. figure:: ../images/setup_lobby4.png
   :alt: Installing the Lobby

You can now navigate to the web interface of the lobby :samp:`https://<FQDN>:9443`.
Please log into the Lobby with the user ``admin`` and the password you chose during the installation:

.. figure:: ../images/setup_lobby5.png
   :alt: Using the Lobby

To configure your Lobby in the ASGARD Management Center,
we will continue later in the chapter :ref:`administration/configuration:Lobby Configuration`.

Broker Installation
^^^^^^^^^^^^^^^^^^^

To install a Broker, run the following command on your newly installed system

.. code-block:: console
   
   nextron@broker:~$ sudo nextronInstaller -broker

.. figure:: ../images/setup_broker1.png
   :alt: Installing a Broker

After the installation is finished, you will see the following message:

.. figure:: ../images/setup_broker2.png
   :alt: Installing a Broker

You can now check if the service was installed successfully.

.. code-block:: console
   
   nextron@broker:~$ systemctl status asgard-broker.service

You will see that the service is in a "**failed/exited**" state.
This will change once we configured our ASGARD with the Broker.

To configure your Broker in the ASGARD Management Center,
we will continue later in the chapter :ref:`administration/configuration:Broker Configuration`.