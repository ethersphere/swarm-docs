RPC
---

Swarm exposes an IPC API under the ``bzz`` namespace.


FUSE
^^^^^^

``swarmfs.mount(HASH|domain, mountpoint))``
  mounts Swarm contents represented by a Swarm hash or a ens domain name to the specified local directory. The local directory has to be writable and should be empty.
  Once this command is succesfull, you should see the contents in the local directory. The HASH is mounted in a rw mode, which means any change insie the directory will be automatically reflected in Swarm. Ex: if you copy a file from somewhere else in to mountpoint, it is equvivalent of using a ``swarm up <file>`` command.    

``swarmfs.unmount(mountpoint)``
  This command unmounts the HASH|domain mounted in the specified mountpoint. If the device is busy, unmounting fails. In that case make sure you exit the process that is using the directory and try unmounting again.

``swarmfs.listmounts()``
  For every active mount, this command display three things. The mountpoint, start HASH supplied and the latest HASH. Since the HASH is mounted in rw mode, when ever there is a change to the file system (adding file, removing file etc), a new HASH is computed. This hash is called the latest HASH.

PSS
^^^^^

``pss`` methods are by default exposed via IPC. If websockets are activated on the node, they will also be available there *if* the ``pss`` module is explicitly specified.

All parameters are hex-encoded bytes or strings unless otherwise noted.

``pss.getPublicKey()``
  Retrieves the public key of the node, in hex format

``pss.baseAddr()``
  Retrieves the Swarm overlay address of the node, in hex format

``pss.stringToTopic(name)``
  Creates a deterministic 4 byte topic value from an input name, returned in hex format

``pss.setPeerPublicKey(publickey, topic, address)``
  Register a peer's public key. This is done once for every topic that will be used with the peer. Address can be anything from 0 to 32 bytes inclusive of the peer's swarm address. The method has no return value.

``pss.sendAsym(publickey, topic, message)``
  Encrypts the message using the provided public key, and signs it using the node's private key. It then wraps it in an envelope containing the topic, and sends it to the network. The method has no return value.

``pss.setSymmetricKey(symkey, topic, address, bool decryption)``
  Register a symmetric key shared with a peer. This is done once for every topic that will be used with the peer. Address can be anything from 0 to 32 bytes inclusive of the peer's Swarm overlay address. If the fourth parameter is false, the key will not be added to the list of symmetric keys used for decryption attempts. The method returns an id used to reference the symmetric key in consecutive calls.

``pss.sendSym(symkeyid, topic, message)``
  Encrypts the message using the provided symmetric key, wraps it in an envelope containing the topic, and sends it to the network. The method has no return value.

``pss.GetSymmetricAddressHint(topic, symkeyid)``
  Return the Swarm address associated with the peer registered with the given symmetric key and topic combination. If a match is found it returns the address data in hex format.

``pss.GetAsymmetricAddressHint(topic, publickey)``
  Return the Swarm address associated with the peer registered with the given asymmetric key and topic combination. If a match is found it returns the address data in hex format.

.. note:: The following methods are used to control the optional pss handshake module. This is an advanced feature, and not required for sending and receiving messages using pss. 

``pss.addHandshake(topic)``
  Activate handshake functionality on the specified topic. The method has no return value.

``pss.removeHandshake(topic)``
  Remove handshake functionality on the specified topic. The method has no return value.

``pss.handshake(publickey, topic, bool block, bool flush)``
  Instantiate handshake with peer, refreshing symmetric encryption keys. If parameter 3 is false the handshake will happen asynchronously. If parameter 4 is true, it will force expiry of all existing keys. The method returns a list of symmetric key ids created by the handshake. If the handshake is asynchronous, however, returned array will be empty.

``pss.getHandshakeKeys(publickey, topic, bool incoming, bool outgoing)``
  Returns the set of valid symmetric encryption keys for a specified peer and topic. If the incoming and outgoing parameters are set, the keys valid for the respective communcations directions are included.

``pss.getHandshakeKeyCapacity(symkeyid)``
  Returns the number of messages (uint16) a symmetric handshake key is valid for.

``pss.getHandshakePublicKey(symkeyid)``
  Returns the public key associated with the specified symmetric handshake key.

``pss.releaseHandshakeKey(publickey, topic, symkeyid, bool instant)`` 
  Invalidate the specified symmetric handshake key. Normally, the key will be kept for a grace period to allow decryption of messages not yet received at the time of release. If the instant parameter is set, this grace period is omitted, and the key removed instantaneously. This method has no return value.


.. uncommentthisChequebook IPC API
.. uncommentthis------------------------------

.. uncommentthisSwarm also exposes an IPC API for the chequebook offering the followng methods:

.. uncommentthis``chequebook.balance()``
.. uncommentthis  Returns the balance of your swap chequebook contract in wei.
.. uncommentthis  It errors if no chequebook is set.

.. uncommentthis``chequebook.issue(beneficiary, value)``
.. uncommentthis  Issues a cheque to beneficiary (an ethereum address) in the amount of value (given in wei). The json structure returned can be copied and sent to beneficiary who in turn can cash it using ``chequebook.cash(cheque)``.
.. uncommentthis  It errors if no chequebook is set.

.. uncommentthis``chequebook.cash(cheque)``
.. uncommentthis  Cashes the cheque issued. Note that anyone can cash a cheque. Its success only depends on the cheque's validity and the solvency of the issuers chequbook contract up to the amount specified in the cheque. The tranasction is paid from your bzz base account.
.. uncommentthis  Returns the transaction hash.
.. uncommentthis  It errors if no chequebook is set or if your account has insufficient funds to send the transaction.

.. uncommentthis``chequebook.deposit(amount)``
.. uncommentthis  Transfers funds of amount  wei from your bzz base account to your swap chequebook contract.
.. uncommentthis  It errors if no chequebook is set  or if your account has insufficient funds.
