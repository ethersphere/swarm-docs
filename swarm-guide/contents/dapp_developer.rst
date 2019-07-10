******************************
Swarm for DAPP-Developers
******************************

Everything what you need to start building your DAPP's on Swarm.

Public gateways
===============

Swarm offers a local HTTP proxy API that Dapps can use to interact with Swarm. The Ethereum Foundation is hosting a public gateway, which allows free access so that people can try Swarm without running their own node.
The Swarm public gateway can be found at https://swarm-gateways.net and is always running the latest Swarm release.

.. important:: Swarm public gateways are temporary and users should not rely on their existence for production services.

.. include:: swarm_dappdev/cli_guide.rst
.. include:: swarm_dappdev/http_guide.rst
.. include:: swarm_dappdev/example_dapps.rst

Advanced Guide - Feeds and More "Working with content"
==================

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

API reference
==================
.. include:: api_reference/available_api.rst
.. include:: api_reference/cli.rst
.. include:: api_reference/http.rst
.. include:: api_reference/javascript.rst
.. include:: api_reference/rpc.rst