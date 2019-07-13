*************************
Swarm for DApp-Developers
*************************

This section is written for developers who want to use Swarm in their own applications.

Swarm offers a **local HTTP proxy** API that dapps or command line tools can use to interact with Swarm. Some modules like `messaging  <PSS>`_ are   only available through RPC-JSON API. The foundation servers on the testnet are offering public gateways, which serve to easily demonstrate functionality and allow free access so that people can try Swarm without even running their own node.

Swarm is a collection of nodes of the devp2p network each of which run the BZZ URL schemes on the same network id.

Public gateways
===============

Swarm offers an HTTP based API that DApps can use to interact with Swarm. The Ethereum Foundation is hosting a public gateway, which allows free access so that people can try Swarm without running their own node.
The Swarm public gateway can be found at https://swarm-gateways.net and is always running the latest Swarm release.

.. important:: Swarm public gateways are temporary and users should not rely on their existence for production services.

.. include:: swarm_dappdev/http_guide.rst
.. include:: swarm_dappdev/cli_guide.rst
.. include:: swarm_dappdev/example_dapps.rst

Working with content (Feeds and more) 
=====================================

In this chapter, we demonstrate features of Swarm related to storage and retrieval. First we discuss how to solve mutability of resources in a content addressed system using the Ethereum Name Service on the blockchain, then using Feeds in Swarm.
Then we briefly discuss how to protect your data by restricting access using encryption.
We also discuss in detail how files can be organised into collections using manifests and how this allows virtual hosting of websites. Another form of interaction with Swarm, namely mounting a Swarm manifest as a local directory using FUSE.
We conclude by summarizing the various URL schemes that provide simple HTTP endpoints for clients to interact with Swarm.

.. include:: advanced_guide/ens.rst
.. include:: advanced_guide/feed.rst
.. include:: advanced_guide/manifests.rst
.. include:: advanced_guide/encryption.rst
.. include:: advanced_guide/act.rst
.. include:: advanced_guide/fuse.rst
.. include:: advanced_guide/bzz.rst

.. include:: messaging/pss.rst


API reference
==================
.. include:: api_reference/http.rst
.. include:: api_reference/javascript.rst
.. include:: api_reference/rpc.rst