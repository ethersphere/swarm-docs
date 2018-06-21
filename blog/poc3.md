

The Swarm Team is pleased to announce the immediate release of Swarm client v0.3, the third proof-of-concept release (POC3) of the Ethereum Swarm client.

Swarm 0.3 has been deployed to the public Testnet, and the Ethereum Foundation is running a 50-node strong cluster of Swarm nodes, together with a public web gateway on https://swarm-gateways.net.

The POC3 code is now merged into the official [go-ethereum repository's master branch](https://github.com/ethereum/go-ethereum).


It has been a year and a half since the first release of the POC2 series was deployed and [the Swarm project launched its public alpha network](https://blog.ethereum.org/2016/12/15/swarm-alpha-public-pilot-basics-swarm/).
Two [Swarm summits](https://swarm-gateways.net/bzz:/swarm-orange-summit.eth), two [orange papers](http://swarm-guide.readthedocs.io/en/latest/resources.html#orange-papers) and [forty thousand lines of code](https://github.com/ethereum/go-ethereum/pull/17041) later, it is time to take stock.



# The past year

In the past year [the Swarm team](https://pbs.twimg.com/media/DetPkqZX0AAAsPp.jpg:large) has grown in size and is now on fire delivering the vision. We have been busy redesigning the network layer, rewriting the retrieval protocol using a stream abstraction, rewriting connectivity management and the overlay network code as well as developed a very sophisticated *network simulation framework*.
POC3 code was finalised just in time for the [Swarm Orange Summit in Ljubljana](https://swarm-gateways.net/bzz:/2018.swarm-summit.eth), where we had 80 participants and a very inspiring and creative week ([watch this two-minute video hosted on Swarm](https://swarm-gateways.net/bzz:/079b4f4155d7e8b5ee76e8dd4e1a6a69c5b483d499654f03d0b3c588571d6be9/)) of talks and coding. It is inspiring to see a growing number of [contributors](https://github.com/ethersphere/go-ethereum/blob/b14d635539a7fd548bd1fe4fe987f137229ff38e/swarm/AUTHORS) and [companies that want to build on swarm](https://swarm-gateways.net/bzz:/swarm-orange-summit.eth/#mu-sponsors)



## Basics of Swarm

Swarm content storage is a lot more than just "bittorrent on steroids". The technical details can be found in the [chapter on architecture](http://swarm-guide.readthedocs.io/en/latest/architecture.html) in the new and improved [Swarm guide](http://swarm-guide.readthedocs.io/en/latest/).

In an [earlier blog post](https://blog.ethereum.org/2016/12/15/swarm-alpha-public-pilot-basics-swarm/), we introduced the basics of Swarm storage and content distribution.

At its core, Swarm is a service that provides APIs to upload and download content to the crowd, and through URL-based addressing offers *virtual hosting of websites* and decentralised applications (dapps) without webservers, using decentralised peer-to-peer distributed infrastructure.  The vision is that of an new internet which is not only fault-tolerant, has zero downtime and offers censorship resistance but is also economically self-sustaining due to a built-in incentive system. By compensating nodes for contributing their bandwidth and disk space, these incentives aim to achieve reliable low-latency scalable retrieval of popular content on the one hand and guarantees persistence of important yet rarely accessed data like archives or backups on the other. For smooth delivery Swarm will use the SWAP protocol (planned for POC3.1) while for storage guarantees it will use a two-tiered insurance system (planned for POC4).

Beyond the basics of data storage and delivery, the POC 3 release includes some new and experimental features.

## PSS

The same p2p connections that are used for data storage and delivery, can also be used for node-to-node messaging. PSS combines Swarm routing (`bzz`)  with the Whisper (`shh`) encrypted message format (`bzz`+`shh`=`pss`). In short, PSS is a [messaging protocol](http://swarm-guide.readthedocs.io/en/latest/pss.html) with strong privacy features running on top of the Swarm network. This messaging infrastructure can be the foundation of a whole new system of internode communication services (the email, tweet, newsletter of the future).

PSS is fully featured yet experimental on the new POC3 network and dapps can interact with it using a [JSON RPC API](http://swarm-guide.readthedocs.io/en/latest/apireference.html#PSS). We are collaborating closely with companies and projects that want to use pss to build mid-layer infrastructure. [Mainframe](http://mainframe.com) is building a slack-alternative collaborative group communications tool ([Onyx](https://blog.mainframe.com/mainframe-pre-alpha-release-fca532317111)) and their web3 SDK, and [Status](http://status.im) have expressed interest in building it into their mobile chat.

## Mutability

Another experimental new feature in POC3 is the Swarm Mutable Resource.
Typical in p2p storage systems, content is addressed by its digital fingerprint (hash) and any changes to the content radically change this address. Users of the web, however, are accustomed to mutable resources: when visiting URL-s we expect to see the most up-to-date version of the 'site'. In order to make it easy to access changing content at predictable addresses, Swarm integrates with the Ethereum Name Service [(ENS)](http://swarm-guide.readthedocs.io/en/latest/usage.html#Ethereum) on the Ethereum blockchain. This is what allows us to to reference Swarm content by names like [bzz://theswarm.eth](https://swarm-gateways.net/bzz:/theswarm.eth/).

Swarm POC3 adds another layer in the form of [*Mutable Resource Updates (MRU)*](http://swarm-guide.readthedocs.io/en/latest/usage.html#Mutable). These allow off-chain updates of content associated with an address at a much faster pace than ENS updates on the blockchain could support, and without incurring the cost of on-chain transactions.

MRU is an experimental feature in current POC3 testnet and is still undergoing changes.

## FUSE support

[FUSE](http://swarm-guide.readthedocs.io/en/latest/usage.html#FUSE), enables users to integrate Swarm data directly into their local file systems (only on Linux and Mac). Using this system, users can "mount a Swarm manifest" as if it were a regular directory. It supports file system read and write operations, in which all content is automatically synced with the Swarm.
In future, combining FUSE with Swarm Mutable Resources, it should be possible, for example, to sync your entire home folder between devices - the backend to a decentralised storage with Dropbox-like functionality.

## Encryption support

Swarm 0.3 comes with built-in [*encryption*](http://swarm-guide.readthedocs.io/en/latest/usage.html#Encryption) allowing for secure uploads of private data. With the planned implementation of *Access Control Trees*, we will have the ability to upload a directory privately and still 'share' a subdirectory with specific peers.

# The year ahead

The year ahead will be both exciting and challenging. As part of the POC3 series, we are planning to switch on a revamped SWAP accounting system (Swarm 0.3.1),
Implementing access control and authentication for Swarm content, and enabling 'light' Swarm nodes (Swarm 0.3.2).

We are on target delivering Swarm POC4 (production beta prerelease) in 2019.

We keep on building a community with our allies who champion the values of web3 and actively collaborate through working groups, building the foundational infrastructure, the backbone of second layer services such as databases (http://wolk.com), private data management (http://datafund.io), rights and creative works licensing (http://jaak.io), decentralised version control (ethergit, http://epiclabs.io), video transcoding and streaming service (http://livepeer.org), and communication and collaboration (https://mainframe.com).

## Contact Us

We welcome any feedback and any contribution. Come find us in our [gitter channel](https://gitter.im/ethereum/swarm) or our [github repository](https://github.com/ethersphere/go-ethereum).
