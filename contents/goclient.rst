******************************
Swarm for Go-Client Contributors
******************************

All what you need to know to contribute to the Swarm Go-Client implementation.

Source code is located at https://github.com/ethersphere/swarm/.

Introduction
================================

Uploaded content is **not guaranteed to persist on the testnet** until storage insurance is implemented (see `Roadmap <https://github.com/orgs/ethersphere/projects/5>`_ for more details). All participating nodes should consider participation a voluntary service with no formal obligation whatsoever and should be expected to delete content at their will. Therefore, users should **under no circumstances regard Swarm as safe storage** until the incentive system is functional.

Swarm offers a **local HTTP proxy** API that dapps or command line tools can use to interact with Swarm. Some modules like `messaging  <PSS>`_ are   only available through RPC-JSON API. The foundation servers on the testnet are offering public gateways, which serve to easily demonstrate functionality and allow free access so that people can try Swarm without even running their own node.

Swarm is a collection of nodes of the devp2p network each of which run the BZZ URL schemes on the same network id.

Swarm nodes can also connect with one (or several) Ethereum blockchains for domain name resolution and one ethereum blockchain for bandwidth and storage compensation.
Nodes running the same network id are supposed to connect to the same blockchain for payments. A Swarm network is identified by its network id which is an arbitrary integer.

Swarm allows for :dfn:`upload and disappear` which means that any node can just upload content to the Swarm and then is allowed to go offline. As long as nodes do not drop out or become unavailable, the content will still be accessible due to the 'synchronization' procedure in which nodes continuously pass along available data between each other.

Swarm supports encryption. Upload of unencrypted sensitive and private data is highly discouraged as **there is no way to undo an upload**. Users should refrain from uploading illegal, controversial or unethical content. 

Always use encryption for sensitive content. For encrypted content, uploaded data is 'protected', i.e. only those that know the reference to the root chunk (the Swarm hash of the file) as well as the decryption key can access the content. Since publishing this reference (on ENS or with Feeds) requires an extra step, users are mildly protected against careless publishing as long as they use encryption. Even though there is no guarantees for removal, unaccessed content that is not explicitly insured will eventually disappear from the Swarm, as nodes will be incentivised to garbage collect it in case of storage capacity limits. 

Swarm is a `Persistent Data Structure <https://en.wikipedia.org/wiki/Persistent_data_structure>`_, therefore there is no notion of delete/remove action in Swarm. This is because content is disseminated to Swarm nodes who are incentivised to serve it.

.. important:: It is not possible to **delete or remove** content uploaded to Swarm. **Always encrypt** sensitive content using the integrated Swarm encryption.

Reporting a bug and contributing
================================

Issues are tracked on github and github only. Swarm related issues and PRs have labels prefixed with *swarm*:

* https://github.com/ethersphere/swarm/issues
* `Good first issues <https://github.com/ethersphere/swarm/issues?utf8=✓&q=is%3Aopen+is%3Aissue+label%3A"good+first+issue">`_

Please include the commit and branch when reporting an issue.

Pull requests should by default commit on the `master` branch.

Prospective contributors please read the `Contributing` section from our readme: https://github.com/ethersphere/swarm#contributing.

The Swarm Go-Client Development Process
================================
[TODO] User-Stories / Epics / Sprint explaination.


Networksimulation-Framework
================================

Find everything you need to run Swarm Networksimulations for testing and debugging.