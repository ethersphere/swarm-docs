

Swarm reaches a significant milestone as the next generation of its proof of concept releases (POC3) is now deployed on the public Testnet. The foundation is running a 50-node strong cluster (with a public gateway on https://swarm-gateways.net) hopefully growing as our collaborators switch on their permanent nodes and clusters.
The POC3 code is now merged into the official [go-ethereum repository's master branch](https://github.com/ethereum/go-ethereum).
It has been a year and a half since the first release of the POC2 series was deployed and [the Swarm project launched its public alpha network](https://blog.ethereum.org/2016/12/15/swarm-alpha-public-pilot-basics-swarm/).
Two [Swarm summits](https://swarm-gateways.net/bzz:/swarm-summit.eth), two [orange papers](http://swarm-guide.readthedocs.io/en/latest/resources.html#orange-papers) and [forty thousand lines of code](https://github.com/ethereum/go-ethereum/pull/17041) later, it is time to give an update.

## Basics of Swarm

Swarm content storage is easiest described to a general audience as "bittorrent on steroids". Here we venture into a more elaborate technical presentation. More detail can be found in the [chapter on architecture in the Swarm guide](http://swarm-guide.readthedocs.io/en/latest/architecture.html).
In an [earlier blog post](https://blog.ethereum.org/2016/12/15/swarm-alpha-public-pilot-basics-swarm/), we introduced the basics of Swarm storage and content distribution. To set the scene for a broader scope, here we recap the most important points.

Swarm is a service that provides APIs to upload and download content and through a url-based addressing offers *virtual hosting of websites* and decentralised applications (dapps) without webservers, using decentralised peer-to-peer distributed infrastructure.
When content is uploaded to Swarm it is chopped up into pieces called *chunks*.
Each chunk is identified with a unique address deterministically derived from its content (using the chunk hash).
The hash addresses of data chunks are themselves packaged into a chunk which in turn has its own hash. In this way the content gets mapped into a Merkle tree, the root hash of which, called the Swarm hash, is used as a *reference* to a file. When this reference then is used to retrieve the file, the client can recursively unpack the tree by fetching the individual chunks and assemble the original content. This hierarchical merklised Swarm hash construct offers *integrity protected random access* into large files;  permitting things like skipping to parts of a video or looking up a key in a database file.

Most importantly, this means that, on the network layer, Swarm only needs to take care of storing and retrieving chunks.
Swarm is a peer-to-peer network of nodes running the Swarm client. The network uses Ethereum's devp2p/rlpx networking layer to run the bzz suite of protocols. Nodes are identified with a Swarm address deterministically derived from the node's Ethereum account public key, which can place them in the same address space as the chunks themselves, so we can define proximity between nodes and chunks.
The semi-permanent TCP connections between peers define an overlay network where messages can be relayed from node to node.
The Swarm network implements a *distributed preimage archive (DPA)*, which is a  type of content addressed distributed chunk store in which nodes closest to the address of a chunk do not only serve information about the content but actually host the data. This DPA is the underlying distributed component of the Swarm service. When you upload content, the chunks need to reach the nodes that are closest to it (a process called *syncing*), so that when you retrieve chunks, nodes can send requests relayed towards the chunk's address via ever closer peers.

The viability of routing hinges on the assumption that any node (uploader/requester) can 'reach' any other node (storer). This can be solved by maintaining a specific network topology, i.e., a connectivity pattern of the overlay network called *kademlia*. Kademlia overlay guarantees the existence of a route between retriever and storer relayed through a number of peers logarithmic in network size. This flavour of overlay is called *forwarding kademlia*.

As retrieve requests are relayed towards the nodes close to the chunk address, the first peer that stores the requested chunk will send the chunk data back to the requesting peer. As the delivery is relayed back towards the requesting node, forwarding peers cache the chunk content, resulting in an *auto scaling elastic cloud*: often-accessed content is replicated throughout the network decreasing its retrieval latency. Nodes are incentivised to do such opportunistic caching as they get rewarded each time they serve a chunk. The *Swarm accounting protocol* allows for an accurate peer-to-peer accounting of such data transfer and can do service for service exchange as well as off-chain payments if balance tilts towards a peer. Caching also results in *maximum resource utilisation* in as much as nodes will fill their dedicated storage space with data passing through them. If capacity is reached, least accessed chunks are purged by a garbage collection process. As a consequence, unpopular content will end up
getting deleted. Storage insurance (to be implemented in POC4 2019) will offer users a secure guarantee to protect important content from being purged.

Since for delivery, each node needs to know only the immediate peer upstream, there is no need to know who originated the request. As a consequence forwarding kademlia allows for "anonymous private browsing" as it were.

The Swarm content service offers an [HTTP interface](http://swarm-guide.readthedocs.io/en/latest/apireference.html#HTTP) via a local proxy running on your local machine and connected to the rest of the swarm. This proxy is used in the browser to serve static assets (such as HTML and Javascript for dynamic content) of dapps. The HTTP API offers [six url schemes]((http://swarm-guide.readthedocs.io/en/latest/usage.html#BZZ) that dapps can use to interact with Swarm content storage.
A [*manifest*](http://swarm-guide.readthedocs.io/en/latest/usage.html#Manifests) is a data structure describing file collections; they specify paths and corresponding content hashes allowing for URL-based content retrieval. The `bzz` URL scheme assumes that the content referenced in the domain is a manifest and renders the content entry whose path matches the one in the request path. This offers the functionality of virtual hosting of websites without dedicated webservers. Hosting on web3 is thus analogous to web2.0, with centralised hosting taken out of the equation.
Manifests can also be mapped to a filesystem directory tree, which allows for uploading and downloading directories. Finally, manifests can also be considered indexes, so it can be used to implement a simple key-value store, or alternatively, a database index.

In sum, the overall vision of Swarm offers a fully decentralised peer-to-peer internet which is not only fault-tolerant, has zero downtime and censorship resistance but also economically self-sustaining due to a built-in incentive system. By compensating nodes for contributing their bandwidth and disk space, these incentives achieve reliable low-latency scalable retrieval of popular content on the one hand and guarantees persistence of important yet rarely accessed data like archives or backups on the other. For smooth delivery Swarm will use the SWAP protocol while for storage guarantees it will use a two-tiered insurance system.

# The year behind

In the past year [the Swarm team](https://pbs.twimg.com/media/DetPkqZX0AAAsPp.jpg:large) has grown in size and now on fire delivering the vision. We have been busy redesigning the network layer, rewriting the retrieval protocol using a stream abstraction, rewriting connectivity management and the overlay network code as well as developed a very sophisticated *network simulation framework*.
This latter feature is a main differentiator of the project and allows us to trial very complex emergent properties of a network of nodes, test algorithmic correctness, fault tolerance and scalability. POC3 code was finalised just for the [Swarm Orange Summit in Ljubljana](https://swarm-gateways.net/bzz:/2018.swarm-summit.eth), where we had 80 participants and a very inspiring and creative week ([watch this two-minute video hosted on Swarm] (https://swarm-gateways.net/bzz:/079b4f4155d7e8b5ee76e8dd4e1a6a69c5b483d499654f03d0b3c588571d6be9/)) of talks and coding. It is inspiring to see a growing number of  [contributors](https://github.com/ethersphere/go-ethereum/blob/b14d635539a7fd548bd1fe4fe987f137229ff38e/swarm/AUTHORS) and [companies that want to build on swarm](http://swarm-guide.readthedocs.io/en/latest/introduction.html#Sponsors)

## Scope

The ambitious vision of a truly decentralised internet implies the existence of base-layer infrastructure for all the things that allows you to run auto-scalable real-time interactive web applications in a trustless peer-to-peer setting. These assume the following components:

* hosting (Swarm)
* rendering (move towards client-side frameworks)
* resource-intensive computation (Truebit, Golem, not discussed here)
* consensus-critical business logic (Ethereum blockchain)
* mutability (ENS, MRU)
* node-to-node messaging (PSS)
* access control (ACT)
* payment and service agreements, escrow and user protection (SW3)
* database services (POT)
* search (like Google, not discussed here)

As we continue finding solutions to these ingredients, Swarm shows remarkable signs of good design as various solutions simply fall out of it often much to our surprise.
In what follows we give an overview of new features in Swarm that are available in POC3.

## PSS

The same routing mechanism that is used when relaying retrieve requests, serving deliveries or syncing chunks can be used to do deterministic node-to-node relayed messaging.
`pss` is the "sound of a swarm whispered" as it were, since it combines Swarm (bzz) routing with whisper (shh) envelopes and topics (`bzz`+`shh`=`pss`) defining a [messaging protocol](http://swarm-guide.readthedocs.io/en/latest/pss.html) with strong privacy features (`pss` as in "hush, mum's the word").
This messaging infrastructure can be the foundation of a whole new system of internode communication services (the email, tweet, newsletter of the future). From this systemic perspective PSS is *Postal Services over Swarm*.

With `pss` you can send messages to any node in the Swarm network.
The messages are routed in the same manner as retrieve requests for chunks.
Due to forwarding kademlia, pss offers sender anonymity.
Since `pss` messages are encrypted (using whisper's crypto), ultimately *the recipient is whoever can decrypt the message*. Encryption can be done using asymmetric or symmetric encryption methods.
Thanks to end-to-end encryption, pss caters for private communication.

Instead of chunk hash reference, pss messages specify a destination in the overlay address space independently of the message payload. This destination can describe a specific node if it is a complete overlay address or a neighbourhood if it is partially specified one. Up to the destination, the message is relayed through devp2p peer connections using forwarding kademlia (passing messages via semi-permanent peer-to-peer TCP connections between relaying nodes using kademlia routing). Within the destination neighbourhood the message is gossiped around (broadcast to everyone). This feature gives pss a sliding scale of darkness:
the larger the destination neighbourhood (the smaller prefix you reveal of the intended recipient overlay address), the more difficult it is to identify the real recipient. As recipient anonymity increases, efficiency (due to using kademlia deterministic routes vs gossip) decreases creating a trade off between anonymity on the one hand and message delivery latency and bandwidth (and therefore cost) on the other. This choice is left to the application.


Using partial addressing, pss offers a sliding scale of recipient anonymity: the larger the destination neighbourhood (the smaller prefix you reveal of the intended recipient overlay address), the more difficult it is to identify the real recipient. On the other hand, since dark routing is inefficient, there is a trade off between anonymity on the one hand and message delivery latency and bandwidth (and therefore cost) on the other. This choice is left to the application.


The message payload is dispatched to message handlers by the recipient nodes and dispatched to subscribers via a JSON-RPC API (typically over web-socket) depending on the topic.
Nodes can communicate privately with each other  even if they are not each others' peers.
Forward secrecy is provided by a 'Diffie-Hellmann handshake' and ephemeral keys.
As a result PSS can maintain arbitrary dark subnetworks in Swarm. These subnetworks indentified by the topics can even handle devp2p protocols.
Not only can pss be used for slow-scale chat messaging, but also for secure file transfer, notifications and collaborative protocols like [CRASH proof of custody]() used in storage incentivisation.

PSS is fully featured yet experimental on the new POC3 network and dapps can interact with it using a [JSON RPC API](http://swarm-guide.readthedocs.io/en/latest/apireference.html#PSS). We are collaborating closely with companies and projects that want to use pss to build mid-layer infrastructure. [Mainframe](http://mainframe.com) is building a slack-alternative collaborative group
 communications tool ([Onyx](https://blog.mainframe.com/mainframe-pre-alpha-release-fca532317111)) and their web3 SDK, and [Status](http://status.im) have expressed interest in building it into their mobile chat.

## Mutability

Hash addressing is immutable in the strong sense that you cannot even express mutable content: "changing the content changes the hash". Users of the web, however, are accustomed to mutable resources: when visiting URL-s we expect to see the most up to date version of the 'site'. Mutable resources on Swarm are made possible by the Ethereum Name Service (ENS)](http://swarm-guide.readthedocs.io/en/latest/usage.html#Ethereum) and [*Mutable Resource Updates (MRU)*](http://swarm-guide.readthedocs.io/en/latest/usage.html#Mutable).
The ENS is a smart contract on the Ethereum blockchain which enables domain owners to register a content reference to their domain.
Using ENS for domain name resolution, the URL scheme provides
content retrieval based on mnemonic (or branded) names, much like the DNS of the world wide web, but without servers.
Mutable Resource Update is, on the other hand, an off-chain solution for communicating updates to a resource, it offers cheaper and potentially faster updates than ENS, yet the updates can be consolidated on ENS by any third party willing to pay for the transaction.

Here is how it works: a publisher creates a mutable resource at time `t` for the name "my daily tweets", that she intends to update every day once (note this in itself is not a promise). By hashing together `ID := SHA3(publisher_pubkey, name, start, period)`, we obtain a so called metadata chunk.
A publisher creates a new version of a mutable resource by serialising `(name, start, period, version)` and signing it.
This simple system is surprisingly powerful and is envisioned to be at the core of implementing group chat, shared FUSE state, and [poor man's consensus](https://www.sharelatex.com/1452913241cqmzrpfpjkym) (soft channel deposit).

MRU is an experimental feature in current POC3 testnet and is still  undergoing changes.

## FUSE support

We mentioned that thanks to manifests, directory trees and collections represented by manifests can be mapped uniquely. This does not only allow upload and download into directory but also thanks to [FUSE](http://swarm-guide.readthedocs.io/en/latest/usage.html#FUSE), enables users to mount a Swarm manifest as a local file system (only available on Linux and Mac).
Due to the fact that Swarm retrieval is on-demand, Swarm FUSE mounts are readable with no latency. Swarm FUSE supports file system write operations (creating, appending, updating files) by maintaining the collection's Swarm hash as an internal state.
Combining FUSE with MRU, on each change we can issue a mutable resource update, this makes it possible to think of FUSE to sync your entire home folder between devices, virtually serving as the backend to a decentralised drop box.

Swarm POC3 supports [*encryption*](http://swarm-guide.readthedocs.io/en/latest/usage.html#Encryption) so protecting your private data has never been easier. Once *Access Control Trees* are implemented, we will have a fully fledged solution for 'sharing' a subdirectory, a feature well known from Dropbox or Google Drive. Combining with PSS the 'share' can be sent in a notification to the recipient.

# The year ahead

Just as the past year, these coming 10+ month will be both exciting and challenging. We are on target delivering Swarm POC4 (production beta prerelease) in the first half of 2019.

As part of the POC3 series, we are planning to switch on a revamped SWAP accounting for chunk deliveries.
Implementing access control and authentication for Swarm content is high on our priority list.
The first few releases will also address some technical debt accumulated as part of the network rewrite as well as optimisations of retrieval and syncing.
After rounding up [the swap swear and swindle framework for decentralised service economies](https://github.com/ethersphere/swap-swear-and-swindle/tree/master) (see the [3rd orange paper]( https://www.sharelatex.com/1452913241cqmzrpfpjkym) and a [talk](https://youtu.be/Bn65-bI-S1o) and the [implementation](https://github.com/ethersphere/swap-swear-and-swindle/tree/master)), we apply it to the storage insurance using the [crash proof of custody scheme](https://swarm-gateways.net/bzz:/theswarm.eth/ethersphere/orange-papers/1/sw^3.pdf).

We are implementing two layers of erasure coding for redundancy, one is primarily against dropout and network switch issues to guarantee low latency retrieval. Another one is part of the two tiered insurance system. On one level, stakeless farmers get positive rewards for proof of custody. On the other level, staked insurers continually perform scan and repair on highly redundantly coded chunks potentially suffering punitive measures as well as needing to pay compensatory insurance to users if integrity of insured content is compromised.

We keep on building a community with our allies who champion the values of web3 and actively collaborate through working groups building the foundational infrastructure, the backbone of second layer services such as databases (http://wolk.com), private data management and data market (http://datafund.io), global view of rights and creative works licensing platform (http://jaak.io), decentralised version control (ethergit, http://epiclabs.io), video transcoding and streaming service (http://livepeer.org).
