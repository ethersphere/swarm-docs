

Swarm
#####

Storage and communication infrastructure for a sovereign digital society.
=========================================================================

..  * extension allows for per-format preference for image format

..  image:: img/swarm.png
   :height: 300px
   :width: 238px
   :scale: 50 %
   :alt: swarm-logo
   :align: left

Swarm is a censorship resistant, permissionles, decentrelized storage- and communication infrastructure. 

This base-layer infrastructure provides these services by contributing resources to each other. These contributions are accurately accounted for on a peer to peer basis, allowing nodes to trade resource for resource, but offering monetary compensation to nodes consuming less than they serve.

Swarm is using existing Smart-Contract platforms (e.g. Ethereum) to implement the financial incentivisation.

.. image:: img/swarm-intro.svg
   :alt: Swarm storage and message routing
   :width: 500
   :align: center

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
| Network ID #1 | EF-Test-Network        | Pubic Test-Network     | Swarm Core-Team / EF Dev OPS | https://swarm-gateways.net |
+---------------+------------------------+------------------------+------------------------------+----------------------------+

The Ethereum Foundation operates a Swarm testnet that can be used to test out functionality in a similar manner to the Ethereum testnetwork.
Everyone can join the network by running the Swarm client.

.. toctree::
   :numbered:
   :maxdepth: 3
   :hidden:

   introduction
   architecture
   swarm_specification
   dapp_developer
   pss
   node_operators
   goclient
   
  
   .. resources


