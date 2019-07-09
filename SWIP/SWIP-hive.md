---
SWIP: <to be assigned>
title: Swarm Network Discovery and Maintenance Protocol
author: Daniel A. Nagy <daniel@ethswarm.org>
status: Draft
type: Standards Track
category: Networking
created: 2019-07-09
requires: EIP-8
---

<!--You can leave these HTML comments in your merged SWIP and delete the visible duplicate text guides, they will not appear and may be helpful to refer to if you edit it again. This is the suggested template for new SWIPs. Note that a SWIP number will be assigned by an editor. When opening a pull request to submit your SWIP, please use an abbreviated title in the filename, `SWIP-draft_title_abbrev.md`. The title should be 44 characters or less.-->

## Simple Summary
<!--"If you can't explain it simply, you don't understand it well enough." Provide a simplified and layman-accessible explanation of the SWIP.-->
Specification of the `hive` protocol used to create and maintain the desired network topology for the Swarm network.

## Abstract
<!--A short (~200 word) description of the technical issue being addressed.-->

Unlike many other Ethereum overlay networks, Swarm actually requires a structured network topology called *Kademlia* for routing data and requests. This document specifies a wire protocol 
implemented as a sub-protocol of Ethereum's RLPx helping Swarm nodes self-organize into a Kademlia network and maintain this topology in the face of nodes joining and leaving the network or 
disconnecting other nodes.

Nodes wishing to join Swarm should find at least *one* node (a so-called *bootstrap node*) that is already a member and use this protocol to find other nodes to which they need to connect. Nodes 
that are already Swarm members will receive and send updates regarding their connectivity over this protocol.

This document also contains the specification of Swarm's flavor of Kademlia.

## Specification
<!--The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for the current Swarm platform and future client implementations. -->

### Forwarding Kademlia

### Bootstrapping

## Rationale
<!--The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.-->

The ad-hoc connections of Ethereum network typically resulting in a scale-free network topology are well suited for rapid block and transaction propagation, which is the network's primary purpose, 
but Swarm's objectives are sufficiently different to warrant a different network topology: unline full Ethereum nodes, Swarm nodes do not store all the information that can be requested from the 
network and therefore it is important to be able to route uploads and retrievale requests to nodes which are responsible for storing that particular piece of information.

Swarm stores content as a DHT [Distributed Hash Table], meaning that database keys and node addresses come from the same address space and there is a distance metric defined over this address space
so that nodes store data with keys that are closer to them according to this metric. Routing retrieval requests and uploaded data towards the nodes that are closest to them should be reliable 
and incur strictly bounded costs for individual nodes as well as for the network as a whole. Kademlia topology has the necessary properties demonstrated both in theory and in practice;
large-scale peer-to-peer networks with Kademlia topology have a track record of successful operation in a complex and hostile environment.

The choice of a *forwarding Kademlia* is motivated by the following three properties:
 * High troughput of TCP connections
 * Compatibility with Ethereum's RLPx connections, providing NAT traversal, initial peer discovery and rudimentary security
 * The high cost of establishing new RLPx connections

## Copyright
Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
