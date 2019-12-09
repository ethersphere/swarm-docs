************************
Swarm for Node-Operators
************************

This section is about how to run your swarm node, or deploy a seperate private or public swarm network.

Installation and Updates
========================

Swarm runs on all major platforms (Linux, macOS, Windows, Raspberry Pi, Android, iOS).

Swarm was written in golang and requires the go-ethereum client **geth** to run.

..  note::
  The swarm package has not been extensively tested on platforms other than Linux and macOS.

Download pre-compiled Swarm binaries
==================================

Pre-compiled binaries for Linux, macOS and Windows are available to download via our `official homepage <https://swarm-gateways.net/bzz:/swarm.eth/downloads/>`_.

Setting up Swarm in Docker
=============================

You can run Swarm in a Docker container. The official Swarm Docker image including documentation on how to run it can be found on `Github <https://github.com/ethersphere/swarm#Docker>`_ or pulled from `Docker <https://hub.docker.com/r/ethersphere/swarm/>`_.

You can run it with optional arguments, e.g.:

.. code-block:: shell

  $ docker run -it ethersphere/swarm --debug --verbosity 4

In order to up/download, you need to expose the HTTP api port (here: to localhost:8501) and set the HTTP address:

.. code-block:: shell

  $ docker run -p 8501:8500/tcp -it ethersphere/swarm  --httpaddr=0.0.0.0 --debug --verbosity 4

In this example, you can use ``swarm --bzzapi http://localhost:8501 up testfile.md`` to upload ``testfile.md`` to swarm using the Docker node, and you can get it back e.g. with ``curl http://localhost:8501/bzz:/<hash>``.

Note that if you want to use a pprof HTTP server, you need to expose the ports and set the address (with ``--pprofaddr=0.0.0.0``) too.

In order to attach a Geth Javascript console, you need to mount a data directory from a volume:

.. code-block:: shell

  $ docker run -p 8501:8500/tcp -v /tmp/hostdata:/data -it --name swarm1 ethersphere/swarm --httpaddr=0.0.0.0 --datadir /data --debug --verbosity 4

Then, you can attach the console with:

.. code-block:: shell

  $ docker exec -it swarm1 geth attach /data/bzzd.ipc

You can also open a terminal session inside the container:

.. code-block:: shell

  $ docker exec -it swarm1 /bin/sh

Installing Swarm from source
=============================

The Swarm source code for can be found on https://github.com/ethersphere/swarm

Prerequisites: Go and Git
--------------------------

Building the Swarm binary requires the following packages:

* go: https://golang.org
* git: http://git.org


Grab the relevant prerequisites and build from source.

.. tabs::

   .. tab:: Ubuntu / Debian

      .. code-block:: shell

         $ sudo apt install git

         $ sudo add-apt-repository ppa:gophers/archive
         $ sudo apt-get update
         $ sudo apt-get install golang-1.11-go

         // Note that golang-1.11-go puts binaries in /usr/lib/go-1.11/bin. If you want them on your PATH, you need to make that change yourself.

         $ export PATH=/usr/lib/go-1.11/bin:$PATH

   .. tab:: Archlinux

      .. code-block:: shell

         $ pacman -S git go

   .. tab:: Generic Linux

      The latest version of Go can be found at https://golang.org/dl/

      To install it, download the tar.gz file for your architecture and unpack it to ``/usr/local``

   .. tab:: macOS

      .. code-block:: shell

        $ brew install go git

   .. tab:: Windows

      Take a look `here <https://medium.freecodecamp.org/setting-up-go-programming-language-on-windows-f02c8c14e2f>`_ at installing go and git and preparing your go environment under Windows.

Configuring the Go environment
-------------------------------

You should then prepare your Go environment.

.. tabs::

    .. group-tab:: Linux

      .. code-block:: shell

        $ mkdir $HOME/go
        $ echo 'export GOPATH=$HOME/go' >> ~/.bashrc
        $ echo 'export PATH=$GOPATH/bin:$PATH' >> ~/.bashrc
        $ source ~/.bashrc

    .. group-tab:: macOS

      .. code-block:: shell

        $ mkdir $HOME/go
        $ echo 'export GOPATH=$HOME/go' >> $HOME/.bash_profile
        $ echo 'export PATH=$GOPATH/bin:$PATH' >> $HOME/.bash_profile
        $ source $HOME/.bash_profile

Download and install Geth
----------------------------------------

Once all prerequisites are met, download and install Geth from https://github.com/ethereum/go-ethereum


Compiling and installing Swarm
----------------------------------------

Once all prerequisites are met, and you have ``geth`` on your system, clone the Swarm git repo and build from source:

.. code-block:: shell

  $ git clone https://github.com/ethersphere/swarm
  $ cd swarm
  $ make swarm

Alternatively you could also use the Go tooling and download and compile Swarm from `master` via:


.. code-block:: shell

  $ go get -d github.com/ethersphere/swarm
  $ go install github.com/ethersphere/swarm/cmd/swarm

You can now run ``swarm`` to start your Swarm node.
Let's check if the installation of ``swarm`` was successful:

.. code-block:: none

  swarm version

If your ``PATH`` is not set and the ``swarm`` command cannot be found, try:

  .. code-block:: shell

    $ $GOPATH/bin/swarm version

This should return some relevant information. For example:

.. code-block:: shell

  Swarm
  Version: 0.3
  Network Id: 0
  Go Version: go1.10.1
  OS: linux
  GOPATH=/home/user/go
  GOROOT=/usr/local/go

Updating your client
---------------------

To update your client simply download the newest source code and recompile.

.. _Getting Started:


Running the Swarm Go-Client
=============

To start a basic Swarm node you must have both ``geth`` and ``swarm`` installed on your machine. You can find the relevant instructions in the `Installation and Updates <./installation.html>`_  section. ``geth`` is the go-ethereum client, you can read up on it in the `Ethereum Homestead documentation <http://ethdocs.org/en/latest/ethereum-clients/go-ethereum/index.html>`_.

To start Swarm you need an Ethereum account. You can create a new account in ``geth`` by running the following command:

.. code-block:: none

  $ geth account new

You will be prompted for a password:

.. code-block:: none

  Your new account is locked with a password. Please give a password. Do not forget this password.
  Passphrase:
  Repeat passphrase:

Once you have specified the password, the output will be the Ethereum address representing that account. For example:

.. code-block:: none

  Address: {2f1cd699b0bf461dcfbf0098ad8f5587b038f0f1}

Using this account, connect to Swarm with

.. code-block:: none

  $ swarm --bzzaccount <your-account-here>
  # in our example
  $ swarm --bzzaccount 2f1cd699b0bf461dcfbf0098ad8f5587b038f0f1

(You should replace ``2f1cd699b0bf461dcfbf0098ad8f5587b038f0f1`` with your account address key).

.. important::

  **Remember your password.** There is no *forgot my password* option for ``swarm`` and ``geth``.

Verifying that your local Swarm node is running
-----------------------------------------------

When running, ``swarm`` is accessible through an HTTP API on port 8500. Confirm that it is up and running by pointing your browser to http://localhost:8500 (You should see a Swarm search box.)

Interacting with Swarm
======================

.. _3.2:

The easiest way to access Swarm through the command line, or through the `Geth JavaScript Console <http://ethdocs.org/en/latest/account-management.html>`_ by attaching the console to a running swarm node. ``$BZZKEY$`` refers to your account address key.

.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY

      And, in a new terminal window:

      .. code-block:: none

        $ geth attach $HOME/.ethereum/bzzd.ipc

    .. group-tab:: macOS

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY

      And, in a new terminal window:

      .. code-block:: none

        $ geth attach $HOME/Library/Ethereum/bzzd.ipc

    .. group-tab:: Windows

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY

      And, in a new terminal window:

      .. code-block:: none

        $ geth attach \\.\pipe\bzzd.ipc


Swarm is fully compatible with Geth Console commands. For example, you can list your peers using ``admin.peers``, add a peer using ``admin.addPeer``, and so on.

You can use Swarm with CLI flags and environment variables. See a full list in the `Configuration <./configuration.html>`_ .

.. _connect-ens:

How do I enable ENS name resolution?
=====================================

The `Ethereum Name Service <http://ens.readthedocs.io/en/latest/introduction.html>`_ (ENS) is the Ethereum equivalent of DNS in the classic web. It is based on a suite of smart contracts running on the *Ethereum mainnet*.

In order to use **ENS** to resolve names to swarm content hashes, ``swarm`` has to connect to a ``geth`` instance that is connected to the *Ethereum mainnet*. This is done using the ``--ens-api`` flag.

First you must start your geth node and establish connection with Ethereum main network with the following command:

.. code-block:: none

  $ geth

for a full geth node, or

.. code-block:: none

  $ geth --syncmode=light

for light client mode.

.. note::

  **Syncing might take a while.** When you use the light mode, you don't have to sync the node before it can be used to answer ENS queries. However, please note that light mode is still an experimental feature.

After the connection is established, open another terminal window and connect to Swarm:

.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ swarm --ens-api $HOME/.ethereum/geth.ipc \
        --bzzaccount $BZZKEY

    .. group-tab:: macOS

      .. code-block:: none

        $ swarm --ens-api $HOME/Library/Ethereum/geth.ipc \
        --bzzaccount $BZZKEY

    .. group-tab:: Windows

      .. code-block:: none

        $ swarm --ens-api \\.\pipe\geth.ipc \
        --bzzaccount $BZZKEY


Verify that this was successful by pointing your browser to http://localhost:8500/bzz:/theswarm.eth/

Using Swarm together with the testnet ENS
------------------------------------------

It is also possible to use the Ropsten ENS test registrar for name resolution instead of the Ethereum main .eth ENS on mainnet.

Run a geth node connected to the Ropsten testnet

.. code-block:: none

  $ geth --testnet

Then launch the ``swarm``; connecting it to the geth node (``--ens-api``).

.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ swarm --ens-api $HOME/.ethereum/geth/testnet/geth.ipc \
        --bzzaccount $BZZKEY

    .. group-tab:: macOS

      .. code-block:: none

        $ swarm --ens-api $HOME/Library/Ethereum/geth/testnet/geth.ipc \
        --bzzaccount $BZZKEY

    .. group-tab:: Windows

      .. code-block:: none

        $ swarm --ens-api \\.\pipe\geth.ipc \
        --bzzaccount $BZZKEY


Swarm will automatically use the ENS deployed on Ropsten.

For other ethereum blockchains and other deployments of the ENS contracts, you can specify the contract addresses manually. For example the following command:

.. code-block:: none

  $ swarm --ens-api eth:<contract 1>@/home/user/.ethereum/geth.ipc \
           --ens-api test:<contract 2>@ws:<address 1> \
           --ens-api <contract 3>@ws:<address 2>

Will use the ``geth.ipc`` to resolve ``.eth`` names using the contract at address ``<contract 1>`` and it will use ``ws:<address 1>`` to resolve ``.test`` names using the contract at address ``<contract 2>``. For all other names it will use the ENS contract at address ``<contract 3>`` on ``ws:<address 2>``.

Using an external ENS source
----------------------------

.. important::

  Take care when using external sources of information. By doing so you are trusting someone else to be truthful. Using an external ENS source may make you vulnerable to man-in-the-middle attacks. It is only recommended for test and development environments.

Maintaining a fully synced Ethereum node comes with certain hardware and bandwidth constraints, and can be tricky to achieve. Also, light client mode, where syncing is not necessary, is still experimental.

An alternative solution for development purposes is to connect to an external node that you trust, and that offers the necessary functionality through HTTP.

If the external node is running on IP 12.34.56.78 port 8545, the command would be:

.. code-block:: none

  $ swarm --ens-api http://12.34.45.78:8545

You can also use ``https``. But keep in mind that Swarm *does not validate the certificate*.


Alternative modes
=================

Below are examples on ways to run ``swarm`` beyond just the default network. You can instruct Swarm using the geth command line interface or use the geth javascript console.

Swarm in singleton mode (no peers)
------------------------------------

If you **don't** want your swarm node to connect to any existing networks, you can provide it with a custom network identifier using ``--bzznetworkid`` with a random large number.


.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY \
        --datadir $HOME/.ethereum \
        --ens-api $HOME/.ethereum/geth.ipc \
        --bzznetworkid <random number between 15 and 256>

    .. group-tab:: macOS

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY \
        --datadir $HOME/Library/Ethereum/ \
        --ens-api $HOME/Library/Ethereum/geth.ipc \
        --bzznetworkid <random number between 15 and 256>

    .. group-tab:: Windows

      .. code-block:: none

        $ swarm --bzzaccount $BZZKEY \
        --datadir %HOMEPATH%\AppData\Roaming\Ethereum \
        --ens-api \\.\pipe\geth.ipc \
        --bzznetworkid <random number between 15 and 256>

Adding enodes manually
------------------------

By default, Swarm will automatically seek out peers in the network.

Additionally you can manually start off the connection process by adding one or more peers using the ``admin.addPeer`` console command.

.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ geth --exec='admin.addPeer("ENODE")' attach $HOME/.ethereum/bzzd.ipc

    .. group-tab:: macOS

      .. code-block:: none

        $ geth --exec='admin.addPeer("ENODE")' attach $HOME/Library/Ethereum/bzzd.ipc

    .. group-tab:: Windows

      .. code-block:: none

        $ geth --exec='admin.addPeer("ENODE")' attach \\.\pipe\bzzd.ipc

(You can also do this in the Geth Console, as seen in Section 3.2_.)

.. note::

  When you stop a node, all peer connections will be saved. When you start again, the node will try to reconnect to those peers automatically.

Where ENODE is the enode record of a swarm node. Such a record looks like the following:

.. code-block:: none

  enode://01f7728a1ba53fc263bcfbc2acacc07f08358657070e17536b2845d98d1741ec2af00718c79827dfdbecf5cfcd77965824421508cc9095f378eb2b2156eb79fa@1.2.3.4:30399

The enode of your swarm node can be accessed using ``geth`` connected to ``bzzd.ipc``

.. tabs::

    .. group-tab:: Linux

      .. code-block:: none

        $ geth --exec "admin.nodeInfo.enode" attach $HOME/.ethereum/bzzd.ipc

    .. group-tab:: macOS

      .. code-block:: none

        $ geth --exec "admin.nodeInfo.enode" attach $HOME/Library/Ethereum/bzzd.ipc

    .. group-tab:: Windows

      .. code-block:: none

        $ geth --exec "admin.nodeInfo.enode" attach \\.\pipe\bzzd.ipc


.. note::
  Note how ``geth`` is used for two different purposes here: You use it to run an Ethereum Mainnet node for ENS lookups. But you also use it to "attach" to the Swarm node to send commands to it.

Connecting to the public Swarm cluster
--------------------------------------

By default Swarm connects to the public Swarm testnet operated by the Ethereum Foundation and other contributors.

The nodes the team maintains function as a free-to-use public access gateway to Swarm, so that users can experiment with Swarm without the need to run a local node. To download data through the gateway use the ``https://swarm-gateways.net/bzz:/<address>/`` URL.

Metrics reporting
------------------

Swarm uses the `go-metrics` library for metrics collection. You can set your node to collect metrics and push them to an influxdb database (called `metrics` by default) with the default settings. Tracing is also supported. An example of a default configuration is given below:

.. code-block:: none

  $ swarm --bzzaccount <bzzkey> \
  --debug \
  --metrics \
  --metrics.influxdb.export \
  --metrics.influxdb.endpoint "http://localhost:8086" \
  --metrics.influxdb.username "user" \
  --metrics.influxdb.password "pass" \
  --metrics.influxdb.database "metrics" \
  --metrics.influxdb.host.tag "localhost" \
  --verbosity 4 \
  --tracing \
  --tracing.endpoint=jaeger:6831 \
  --tracing.svc myswarm

Go-Client Command line options Configuration
====================================

.. _configuration:

The ``swarm`` executable supports the following configuration options:

* Configuration file
* Environment variables
* Command line

Options provided via command line override options from the environment variables, which will override options in the config file. If an option is not explicitly provided, a default will be chosen.

In order to keep the set of flags and variables manageable, only a subset of all available configuration options are available via command line and environment variables. Some are only available through a TOML configuration file.

.. note:: Swarm reuses code from ethereum, specifically some p2p networking protocol and other common parts. To this end, it accepts a number of environment variables which are actually from the ``geth`` environment. Refer to the geth documentation for reference on these flags.

This is the list of flags inherited from ``geth``:

.. code-block:: none

  --identity
  --bootnodes
  --datadir
  --keystore
  --port
  --nodiscover
  --v5disc
  --netrestrict
  --nodekey
  --nodekeyhex
  --maxpeers
  --nat
  --ipcdisable
  --ipcpath
  --password

Config File
=============

.. note:: ``swarm`` can be executed with the ``dumpconfig`` command, which prints a default configuration to STDOUT, and thus can be redirected to a file as a template for the config file.

A TOML configuration file is organized in sections. The below list of available configuration options is organized according to these sections. The sections correspond to `Go` modules, so need to be respected in order for file configuration to work properly. See `<https://github.com/naoina/toml>`_ for the TOML parser and encoder library for Golang, and `<https://github.com/toml-lang/toml>`_ for further information on TOML.

To run Swarm with a config file, use:

.. code-block:: shell

  $ swarm --config /path/to/config/file.toml

General configuration parameters
================================

.. csv-table::
   :header: "Config file", "Command-line flag", "Environment variable", "Default value", "Description"
   :widths: 10, 5, 5, 15, 55

   "n/a","--config","n/a","n/a","Path to config file in TOML format"
   "n/a","--bzzapi","n/a","http://127.0.0.1:8500","Swarm HTTP endpoint"
   "BootNodes","--bootnodes","SWARM_BOOTNODES","","Boot nodes"
   "BzzAccount","--bzzaccount","SWARM_ACCOUNT", "","Swarm account key"
   "BzzKey","n/a","n/a", "n/a","Swarm node base address (:math:`hash(PublicKey)hash(PublicKey))`. This is used to decide storage based on radius and routing by kademlia."
   "Cors","--corsdomain","SWARM_CORS", "","Domain on which to send Access-Control-Allow-Origin header (multiple domains can be supplied separated by a ',')"
   "n/a","--debug","n/a","n/a","Prepends log messages with call-site location (file and line number)"
   "n/a","--defaultpath","n/a","n/a","path to file served for empty url path (none)"
   "n/a","--delivery-skip-check","SWARM_DELIVERY_SKIP_CHECK","false","Skip chunk delivery check (default false)"
   "EnsApi","--ens-api","SWARM_ENS_API","<$GETH_DATADIR>/geth.ipc","Ethereum Name Service API address"
   "EnsRoot","--ens-addr","SWARM_ENS_ADDR", "ens.TestNetAddress","Ethereum Name Service contract address"
   "ListenAddr","--httpaddr","SWARM_LISTEN_ADDR", "127.0.0.1","Swarm listen address"
   "n/a","--manifest value","n/a","true","Automatic manifest upload (default true)"
   "n/a","--mime value","n/a","n/a","Force mime type on upload"
   "NetworkId","--bzznetworkid","SWARM_NETWORK_ID","3","Network ID"
   "Path","--datadir","GETH_DATADIR","<$GETH_DATADIR>/swarm","Path to the geth configuration directory"
   "Port","--bzzport","SWARM_PORT", "8500","Port to run the http proxy server"
   "PublicKey","n/a","n/a", "n/a","Public key of swarm base account"
   "n/a","--recursive","n/a", "false","Upload directories recursively (default false)"
   "n/a","--stdin","","n/a","Reads data to be uploaded from stdin"
   "n/a","--store.path value","SWARM_STORE_PATH","<$GETH_ENV_DIR>/swarm/bzz-<$BZZ_KEY>/chunks","Path to leveldb chunk DB"
   "n/a","--store.size value","SWARM_STORE_CAPACITY","5000000","Number of chunks (5M is roughly 20-25GB) (default 5000000)]"
   "n/a","--store.cache.size value","SWARM_STORE_CACHE_CAPACITY","5000","Number of recent chunks cached in memory (default 5000)"
   "n/a","--sync-update-delay value","SWARM_ENV_SYNC_UPDATE_DELAY","","Duration for sync subscriptions update after no new peers are added (default 15s)"
   "SyncDisabled","--nosync","SWARM_ENV_SYNC_DISABLE","false","Disable Swarm node synchronization"
   "SwapBackendURL","--swap-backend-url","SWARM_SWAP_BACKEND_URL","","URL of the Ethereum API provider to use to settle SWAP payments"
   "SwapEnabled","--swap","SWARM_SWAP_ENABLE","false","Enable SWAP"
   "SwapPaymentThreshold","--swap-payment-threshold","SWARM_SWAP_PAYMENT_THRESHOLD","1000000","honey amount at which payment is triggered"
   "SwapDisconnectThreshold","--swap-disconnect-threshold","SWARM_SWAP_DISCONNECT_THRESHOLD","1500000","honey amount at which a peer disconnects"
   "SwapDepositAmount","--swap-deposit-amount","SWARM_SWAP_DEPOSIT_AMOUNT","0","Deposit amount for swap chequebook"
   "SwapSkipDeposit", "swap-skip-deposit", "SWARM_SWAP_SKIP_DEPOSIT", "false", "Don't deposit during boot sequence"
   "SwapLogPath","--swap-audit-logpath","SWARM_SWAP_LOG_PATH","n/a","Write execution logs of swap audit to the given directory"
   "SwapChequebookFactory", "swap-chequebook-factory", "SWARM_SWAP_CHEQUEBOOK_FACTORY_ADDR", "ropsten: 0x878Ccb2e3c2973767e431bAec86D1EFd809480d5", "SWAP chequebook factory contract address (default value defined per blockchain network)"
   "Contract","--swap-chequebook","SWARM_CHEQUEBOOK_ADDR","0x0","Swap chequebook contract address"
   "n/a","--verbosity value","n/a","3","Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail"
   "n/a","--ws","n/a","false","Enable the WS-RPC server"
   "n/a","--wsaddr value","n/a","localhost","WS-RPC server listening interface"
   "n/a","--wsport value","n/a","8546","WS-RPC server listening port"
   "n/a","--wsapi value","n/a","n/a","API's offered over the WS-RPC interface"
   "n/a","--wsorigins value","n/a","n/a","Origins from which to accept websockets requests"
   "n/a","n/a","SWARM_AUTO_DEFAULTPATH","false","Toggle automatic manifest default path on recursive uploads (looks for index.html)"
