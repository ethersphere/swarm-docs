.. important::
  **New Bee client**

  In the effort to release a production-ready version of Swarm, the Swarm dev team has migrated their effort to build the new `Bee client <https://github.com/ethersphere/bee>`_, a brand-new implementation of Swarm. The main reason for this switch was the availability of a more mature networking layer (`libp2p <https://docs.libp2p.io/>`_) and the secondary reason being that the insight gained from developing Swarm taught us many lessons which can be implemented best from scratch. While Bee is currently not exposing all features you got used to in Swarm, the development is happening at lightspeed and soon, it will surpass Swarm in functionality and stability!

  Please refer to `Swarm webpage <https://swarm.ethereum.org/>`_ for more information about the state of Bee client and to `Bee documentation <https://docs.ethswarm.org>`_ for documentation.

  **Old Swarm client**

  Old Swarm client, described by this documentation, can still be used until the network exists, however no maintenance or upgrades are planned for it.

  **Compatibility of Bee with the first Swarm**

  Ethereum Swarm Bee is the second official Ethereum Swarm implementation. No compatibility on the network layer with the first Ethereum Swarm implementation can be provided, mainly because the change in underlying network protocol from devp2p to libp2p. This means that a Bee node cannot join first Swarm network and vice versa. Migrating data is possible, please refer to `Bee documentation <https://docs.ethswarm.org>`_.


.. raw:: html

  <div style="border: 1px solid black; padding: 15px; font-size: 1.2em; margin-bottom: 40px; font-weight:bold; background-color: lightgrey">
  Storage and communication infrastructure for a sovereign digital society.
  </div>



Introduction
************



..  * extension allows for per-format preference for image format

..  image:: img/swarm.png
   :height: 300px
   :width: 238px
   :scale: 50 %
   :alt: swarm-logo
   :align: left

Swarm is a censorship resistant, permissionless, decentralised storage and communication infrastructure.

This base-layer infrastructure provides these services by contributing resources to each other. These contributions are accurately accounted for on a peer to peer basis, allowing nodes to trade resource for resource, but offering monetary compensation to nodes consuming less than they serve.

Swarm is using existing Smart-Contract platforms (e.g. Ethereum) to implement the financial incentivisation.

.. raw:: html

  <iframe style="margin-bottom: 40px" width="560" height="315" src="https://www.youtube.com/embed/VgTZV471WFM?start=192" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




Implementations of Swarm
========================

+------------------------+------------------------+----------------------------+--------------------------------------------+
|Client-Name             |Programming Language    |Maintained by               | Notes                                      |
+========================+========================+============================+============================================+
|Swarm Go-Client         | golang                 |Swarm Core-Team             |                                            |
+------------------------+------------------------+----------------------------+--------------------------------------------+
|Swarm Nim-Client        | nim                    |Status.im                   | Development is planned to start Q1 2020    |
+------------------------+------------------------+----------------------------+--------------------------------------------+

Public Swarm Networks
=====================

+---------------+------------------------+------------------------+------------------------------+----------------------------+
| Network ID    |Network Name            | Type of Network        | Maintained by                | Gateways                   |
+===============+========================+========================+==============================+============================+
| Network ID #4 | EF-Test-Network        | Public Test-Network     | Swarm Core-Team / EF Dev OPS | https://swarm-gateways.net |
+---------------+------------------------+------------------------+------------------------------+----------------------------+

The Ethereum Foundation operates a Swarm testnet that can be used to test out functionality in a similar manner to the Ethereum testnetwork.
Everyone can join the network by running the Swarm client.

Development status
==================

Uploaded content is **not guaranteed to persist on the testnet** until storage insurance is implemented. All participating nodes should consider participation a voluntary service with no formal obligation whatsoever and should be expected to delete content at their will. Therefore, users should **under no circumstances regard Swarm as safe storage** until the incentive system is functional.
