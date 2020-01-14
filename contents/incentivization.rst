.. _incentivization:

**********************
Incentives system
**********************

Cryptoeconomics
================
Cryptoeconomics is usually (and somewhat obviously) understood as the intersection of cryptography with economy [https://en.wikiversity.org/wiki/Cryptoeconomics]. The cryptographic part in a broader sense encompasses the vast world of cryptocurrencies, blockchain networks and secure digital decentralized systems, while the economics side generally deals with economic *incentives* for network participant. The basic principle is to reward individual network participants in some way for contributing resources to the network at large, with the aim to  ensure smooth and successful operation of a network, guarantee protocol execution, and fulfill end users' requirements.

Every project with cryptoeconomic elements defines its own rules, protocols and incentives system to reward users, which reflect the different needs and operating principles of the specific network.

For Swarm to properly function as a decentralized p2p storage and communication infrastructure, on very basic terms its peers need to:
 * contribute bandwidth for incoming and outgoing requests
 * provide storage for users to upload and retrieve data
 * forward incoming requests to peers who can fulfill them if they can not serve the request themselves

Swarm introduces its own incentives system for ensuring correct network behavior by rewarding nodes for serving these functions. The general swarm docs specify different functional layers of Swarm, which roughly correspond to its implementation roadmap:
 * accounting system
 * file insurance
 * litigation

Accounting
==========
At the core of Swarm's incentive system is the SWarm Accounting Protocol, or SWAP. It is a very simple protocol which relies on a tit-for-tat p2p accounting model:
 * a node gets rewarded for serving resources
 * a node gets charged for requesting resources

Every peer maintains a local database on its own file system which contains accounting information of all peers with which it exchanged data. 

.. note::

   SWAP excels at its simplicity. It is not the goal and would completely defeat the purpose of the system to introduce bullet-proof consistency and auditing - Swarm is not a blockchain in its own terms.`


.. important::
   The Swarm node operator is responsible for maintenance of the local accounting database (a LevelDB instance). Tampering with it, altering its contents or removing it altogether may result in the node becoming inoperable.

If a node requests data from a peer, when the peer delivers the data, the peer credits an amount equivalent to the price of that message in its local database, at the index represting the peer. The node, when it receives the message (data) from the peer, debits itself in its local database the same amount at the index of the peer. 

.. image:: img/swap.svg
   :alt: Simple SWAP exchange between two nodes 
   :width: 400

In the above example, A requested data from B, which B then delivers to A (could be a chunk). Assuming a price of 200 units for the chunk, B credits itself in its local database plus 200 at the index for node A, while A, when it finally receives the data from B, debits minus 200 in its local database at the index for node B.

Due to the distributed nature of Swarm nodes, entries in the local databases do not happen simultaneously, and there is no notion of transactions nor confirmations; in normal operation, both nodes account for the message with the correspondent node at the moment of the action (for the sending peer, at the moment of sending, for the receiving peer, at the moment of receiving). 

Fraud risks
-----------
Of course, this means that nodes can alter their database and pretend to have different balances to other nodes. The simplicity of this mutual accounting though effectively eliminates fraud, as if node A modifies its entry with B, at settlement time, B will verify in its local database that the claim is not matched with its records and simply ignores fraudulent claims. Normal behavior is to disconnect a node in this case.

Imbalances
----------
Imbalances between nodes more generally leads to disconnects from peers. The downside of this is that if node A was able to send an accounted message to B, which successfully left A, but for some reason never arrived at B, then this would lead to imbalances as well. Swarm currently treats this case as an edge case and does not implement any balance synchronization nor clearance protocol to address such cases. It may though be considered for the future.

Settlement with cheques
------------------------
The balance entries for each node in local databases represent just accounting entries in Swarm's internal accounting unit, but are just numbers. The Swarm papers document the notion of a threshold at which finally a financial settlement protocol is initiated. If a node's balance with a peer crosses the payment threshold, which is a number every peer can set individually, but has a reasonable default defined in the code, then the node kicks off the settlement process. This process involves a series of security and sanity checks, culminating in sending a **signed cheque** to its peer. This signed cheque is a piece of data containing the amount, the source chequebook address and the beneficiary chequebook address, as well as thesignature of the cheque issuer. The peer, upon receiving the cheque, will initiate a cashing transaction trying to cash the cheque in - this is a transaction on a blockchain and represents real financial value. If the cheque was valid and backed by funds, and thus results in a real transfer of funds from the issuer's contract address to the beneficiary's, the peer will regard the transaction as succesful and reset the balance with the node by the cheque's amount. 

PricedMessage
=============

Postage Stamps
==============

