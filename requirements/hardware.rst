.. index:: Hardware Requirements

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
     - \- CPU Cores: 1
      
       \- System memory: 4 GB
       
       \- Hard Disk: 80 GB
   * - up to 10,000
     - \- CPU Cores: 4
      
       \- System memory: 6 GB
      
       \- Hard Disk: 200 GB
   * - up to 25,000
     - \- CPU Cores: 10

       \- System memory: 16 GB
      
       \- Hard Disk: 500 GB

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
your `ASGARD Management Center <https://asgard-manual.nextron-systems.com/en/latest/requirements/hardware.html>`_,
apart from the disk space. Please orientate yourself on the configuration
of your ASGARD. The recommendations are the following:

.. list-table::
   :header-rows: 1
   :widths: 30, 70

   * - Connected Endpoints
     - Minimum  Hardware Requirements
   * - up to 500
     - \- System memory: 4 GB
       
       \- Hard disk: 200 GB
       
       \- CPU Cores: 2
   * - up to 10,000
     - \- System memory: 8 GB
      
       \- Hard disk: 250 GB
       
       \- CPU Cores: 4
   * - up to 25,000
     - \- System memory: 16 GB
      
       \- Hard disk: 300 GB
       
       \- CPU Cores: 4

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
