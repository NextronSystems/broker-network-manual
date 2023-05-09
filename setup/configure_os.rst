.. index:: Configure OS

Changing the IP-Address
-----------------------

You components IP Addresses can be changed in **/etc/network/interfaces**. The IP is configured with the address variable.

.. code-block:: console

   nextron@asgard:~$ sudo vi /etc/network/interfaces

.. code-block::

   auto ens32
   iface ens32 inet static
   address 192.0.2.7
   netmask 255.255.255.0
   gateway 192.0.2.254

.. note::
   There might be a case where the name of the network interface (in this example: ``ens32``) is different.
   To verify this you can run ``ip a`` and see the name of the network interface.

The new IP can be applied with the command ``sudo systemctl restart networking``.

Make sure to update the A-Records in your local DNS Server to reflect the IP changes.

Verifying DNS Settings
^^^^^^^^^^^^^^^^^^^^^^

To verify if your components are using the correct DNS Server, you can inspect the file ``/etc/resolv.conf``:

.. code-block:: console

   nextron@asgard-ac:~$ cat /etc/resolv.conf 
   search example.org
   nameserver 172.16.200.2

If you see errors in this configuration, you can change it with the following command:

.. code-block:: console

   nextron@asgard-ac:~$ sudoedit /etc/resolv.conf