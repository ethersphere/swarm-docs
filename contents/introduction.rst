*******************
Introduction
*******************

=================
Swarm
=================

..  * extention allows for per-format preference for image format

..  image:: img/swarm-logo.jpg
   :height: 300px
   :width: 300 px
   :scale: 50 %
   :alt: swarm-logo
   :align: right


This document presents @value{SWARM} which is being developed as part
of the crypto 2.0 vision for web 3.0.

Swarm is a decentralised document storage solution. It is deeply integrated within
ethereum and provides one of the most prominent base layer service: data storage
and content distribution. It is free and self-sustaining via its incentive structure
implemented as ethereum smart contracts.

This document provides you with:

* information on how to run and configure a local swarm node
* how to connect to the test network
* how to store and access content on swarm
* basic intro to the swarm architecture and basic concepts like swarm hash and swarm manifest
* command line tools relevant to swarm
* API documentation for the http swarm proxy
* API documentation for the bzz RPC module
* how to register swarm domains with the Ethereum Name Service
* how to debug and report issues

Background
=================

The primary objective of Swarm is to provide a sufficiently
decentralized and redundant store of Ethereum's public record, in
particular to store and distribute Đapp code and data as well as
block chain data. [Note that the latter is not part of the current release].

From an economic point of view, it allows participants to efficiently
pool their storage and bandwidth resources in order to provide the
aforementioned services to all participants.

These objectives entail the following design requirements:

.. this list is confusing. what is it a list of? what do "inclusivity" or "self-managed sustainability" mean? does the reader know?
.. TODO: reformulate?

* distributed storage, inclusivity, long tail of power law
* flexible expansion of space without hardware investment decisions, unlimited growth
* zero downtime
* immutable unforgeable verifiable yet plausibly deniable
* no single point of failure, fault and attack resilience
* censorship resistance, permanent public record
* self-managed sustainability via incentive system
* efficient market driven pricing. tradeable trade off of memory, persistent storage, bandwidth
* efficient use of the blockchain by swarm accounting protocol
* deposit-challenge based guaranteed storage [planned for future release]

Basics
========================



Swarm client is part of the Ethereum stack, the reference implementation is currently at POC (proof of concept) version 0.2.

Swarm defines the bzz subprotocol running on the ethereum devp2p network. The bzz subprotocol is in flux, the
specification of the wire protocol is considered stable only with POC 0.4 expected in Q2 2017.

The swarm of Swarm is the collection of nodes of the devp2p network each of which run the bzz protocol on the same network id.

Swarm nodes are also connected to an ethereum blockchain.
Nodes running the same network id are supposed to connect to the same blockchain.
Such a swarm network is identified by its network id which is an arbitrary integer.

Swarm allows for 'upload and disappear' which means that any node can just upload content to the swarm and
then is allowed to go offline. As long as nodes do not drop out or become unavailable, the content will still
be accessible due to 'syncronisation' procedure in which node continuously pass along available data between each other.

.. note::
  Uploaded content is not guaranteed to persist until storage insurance is implemented (expected in POC 0.4). All participating nodes should be considered to contribute voluntary service with no formal obligation whatsoever and should be expected to delete content at their will. Therefore, users should under no circumstances regard swarm as safe storage.

.. note::
  Swarm POC 0.2 uses no encryption. Upload of sensitive and private data is highly discouraged as there is no way to undo an upload. Users should refrain from uploading unencrypted sensitive data, in other words

  * no valuable personal content
  * no illegal, controversial or unethical content

Data structure and naming
-------------------------

Swarm defines 3 crucial notions

* chunks - pieces of data in the swarm
* hash - cryptographic hash of data that serves as its unique identifier and address
* manifests - allow for higher-level collections of data that are semantic to the user

In this guide, content is understood very broadly in a technical sense denoting any blob of data.
Swarm defines a specific identifier for a piece of content. This identifier serves as the retrieval address of the content.
Identifiers need to be

* collision free (two different blobs of data will never map to the same identifier)
* deterministic (same content will always receive the same identifier)
* uniformly distributed

The choice of identifier in swarm POC0.3 is the hierarchical swarm hash described in :ref:`swarm hash`.
The properties above let us view the identifiers as addresses at which content is expected to be found.
Since hashes can be assumed to be collision free, they are bound to one specific version of a content, i.e. Hash addressing therefore is immutable in the strong sense that you cannot even express mutable content: "changing the content, changes the hash".

Users however usually use some discovery and or semantic access to data, which is implemented by the ethereum name service (ENS).
The ENS enables content retrieval based on mnemonic (or branded) names, much like the DNS of WWW, but without servers.

Swarm nodes participating in the network also have their own base address. These node addresses define a location in the same address space as the data. 

When content is uploaded to swarm it is chopped up into pieces called *chunks*. Each chunk is accessed at the address defined by its *swarm hash*. The hashes of data chunks themselves are packaged into a chunk which in turn has its own hash. In this way the content gets maps to a chunk tree. This hierarchical swarm hash construct allows for merkle proofs for chunks within a piece of content, thus providing swarm with integrity protected random access into (large) files.

The current version of swarm implements a strictly content addressed distributed hash table. Here 'strictly content addressed' means that the node(s) closest to the address of a chunk do not only serve information about the content but actually host the data. (Note that although it is part of the protocol, we cannot have any sort of guarantee that it will be preserved. this is a caveat worth stating again: no guarantee of permanence and persistence). In other words, in order to retrieve a piece of content (as a part of a larger collection/document) a chunk must reach its destination from initiator to the storer after upload as well must be served back to the requester at download.
The viability of both hinges on the assumption that any node (requester) can 'reach' any other node (storer). This assumption is guaranteed with a special network topology (called kademlia), which offers (very low) constant time for lookup usually logarithmic to the network size.

Swarm content access is centred around the notion of a manifest. A manifest file describes a document collection, e.g.,

* a filesystem directory
* an index of a database
* a virtual server

Manifests specify paths and corresponding content hashes allowing for url based content retrieval.
Manifests can therefore define a routing table for (static) assets (including dynamic content using the static javascript).
This offers the functionality of virtual hosting, storing entire directories or web(3)sites, similar to www but
without servers.

You can read more about these components in :ref:`Architecture`.

About
===================

This document
---------------------

This document source code is found at @url{https://github.com/ethersphere/swarm/tree/master/book}
The most up-to-date swarm book in various formats is available on the old web
@url{http://ethersphere.org/swarm/docs} as well as on swarm @url{bzz://swarm/docs}


Status
---------------

The status of swarm is proof of concept vanilla prototype tested on a toy network.
It is highly experimental code and untested in the wild.
Use with extreme care.

License
-------------

Swarm is free software.

It is licensed under @dfn{LGPL}, which roughly means the following.

There are @emph{no restrictions on downloading} it other than
your bandwidth and our slothful ways of making things available.

There are @emph{no restrictions on use} either other than its deficiencies,
clumsy features and outragous bugs. However, this can be amended,
because there are @i{no restrictions on modifying} it either.
See also @ref{Contributing}.

Freedom of use implies that anything goes.

What is more, there are @i{no restrictions on redistributing} this software or
any modified version of it.

For some legalese telling you the same, read the License @c
@uref{http://creativecommons.org/licenses/LGPL/2.1/}

@c Creative Commons.

@c @ref{Creative Commons}.

Credits
---------------------

Swarm is code by Ethersphere (ΞTHΞRSPHΞЯΞ), the team behind swarm: Viktor Trón, Dániel A. Nagy and Zsolt Felföldi.

Swarm is funded by the Ethereum Foundation.

Special thanks to

* Felix Lange, Alex Leverington for inventing and implementing devp2p/rlpx;
* Jeffrey Wilcke and the go team for continued support, testing and direction;
* Gavin Wood and Vitalik Buterin for the vision;
* Alex Van der Sande, Fabian Vogelsteller and Dániel Varga for a lot of inspiring discussions and ideas, shaping design from early on;
* Nick Johnson for
* Aron Fischer for his ideas and hands-on help with analysis, documentation and testing
* Roman Mandeleil and Anton Nashatyrev for the java implementation;

Community
-------------------

Daily development and discussions are ongoing in various gitter channels:

* https://gitter.im/ethereum/swarm: general public chatroom about swarm dev
* https://gitter.im/ethersphere/orange-lounge: our reading/writing/working group and R&D sessions
* https://gitter.im/ethereum/pss: about postal services on swarm - messaging with deterministic routing
* https://gitter.im/ethereum/
* https://gitter.im/ethereum/swarm
* Reddit: http://www.reddit.com/r/ethereum

Reporting a bug
-------------------

Issues are tracked on github and github only: @url{https://github.com/ethereum/go-ethereum/labels/swarm}

See the ethereum developer's guide for how to submit a bug report, feature request or fix: https://github.com/ethereum/go-ethereum/wiki/Developers'-Guide

Contributing
--------------------

Testing one library:

```
godep go test -v -cpu 4 ./eth
```

Using options `-cpu` (number of cores allowed) and `-v` (logging even if no error) is recommended.

Testing only some methods:

```
godep go test -v -cpu 4 ./swarm/n -run TestMethod
```

**Note**: here all tests with prefix _TestMethod_ will be run, so if you got TestMethod, TestMethod1, then both!

Running benchmarks, eg.:

```
cd swarm/storage
godep go test -v -cpu 4 -bench . -run BenchmarkJoin
```

for more see [go test flags](http://golang.org/cmd/go/#hdr-Description_of_testing_flags)

See integration testing information on the [Testing wiki page](https://github.com/ethereum/go-ethereum/wiki/Testing)

### Metrics and monitoring

`geth` can do node behaviour monitoring, aggregation and show performance metric charts.
Read about [metrics and monitoring](https://github.com/ethereum/go-ethereum/wiki/Metrics-and-Monitoring)

### Add and update dependencies

To update a dependency version (for example, to include a new upstream fix), run

```
go get -u <foo/bar>
godep update <foo/...>
```

To track a new dependency, add it to the project as normal than run

```
godep save ./...
```

Changes to the [Godeps folder](https://github.com/ethereum/go-ethereum/tree/develop/Godeps) should be manually verified then committed.

To make life easier try [git flow](http://nvie.com/posts/a-successful-git-branching-model/) it sets this all up and streamlines your work flow.

## Contributing

Only github is used to track issues. (Please include the commit and branch when reporting an issue.)

Pull requests should by default commit on the `develop` branch.
The `master` branch is only used for finished stable major releases.

## Stacktrace

The code uses `pprof` on localhost port 6060 by default if `geth` is started with the `--pprof` option. So bring up http://localhost:6060/debug/pprof to see the heap, running routines etc. By clicking full goroutine stack dump (clicking http://localhost:6060/debug/pprof/goroutine?debug=2) you can generate trace that is useful for debugging.

Note that if you run multiple instances of `geth`, this port will only work for the first instance that was launched. If you want to generate stacktraces for these other instances, you need to start them up choosing an alternative pprof port. Make sure you are redirecting stderr to a logfile.

```
geth -port=30300 -loglevel 5 --pprof --pprofport 6060 2>> /tmp/00.glog
geth -port=30301 -loglevel 5 --pprof --pprofport 6061 2>> /tmp/01.glog
geth -port=30302 -loglevel 5 --pprof --pprofport 6062 2>> /tmp/02.glog
```

Alternatively if you want to kill the clients (in case they hang or stalled synching, etc) but have the stacktrace too, you can use the `-QUIT` signal with `kill`:

```
killall -QUIT geth
```

This will dump stracktraces for each instance to their respective log file.

## Code formatting

Sources are formatted according to the [Go Formatting
Style](http://golang.org/doc/effective_go.html#formatting).

Roadmap
-------------------

For actual issues, see https://github.com/ethereum/go-ethereum/labels/swarm
* SWAP^3: swarm accounting protocol stage 3 adding debt swap (accreditation)
* SWEAR & SWINDLE storage incentives: receipts and litigation
* SWORD ethereum blockchain, state, contract storage, logs and receipts on swarm
* network stress testing, viability, scalability
* latency and traffic simulations for routing
* encryption for basic PD masking
* proveable prefix array for full text search,
* swarm db, swarm fs via fuse

Resources
----------------

Talks:

* Dr. Daniel A. Nagy: Keeping the Public Record Safe and Accessible. Ethereum ÐΞVCON0, Berlin. 2014 - @url{https://www.youtube.com/watch?v=QzYZQ03ON2o}
* Viktor Trón, Daniel A. Nagy: Swarm. ÐΞVCON1, London. 2015

