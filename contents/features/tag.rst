
Tags
-----

Tags are meant as a complementary component to track the state of an upload on Swarm. Tags consist mainly of counters and their sole purpose is to track and expose information necessary to display the progress of your uploads.
Whenever you upload content, a tag is automatically created in order to allow tracking of your upload in its various stages.

The tag API is supported through two different interfaces: CLI and HTTP.


CLI
^^^^

The tag API can will displayed through the command line interface through the ``swarm up`` command.
When uploading a file, the status will be shown by default with a simple progress bar. This uses tags under the hood through the HTTP server:

.. code-block:: none
  
  $ echo "this is an example" > example.md
  $ swarm up example.md
    Swarm Hash: 730c96f6de2b5b3b961b3cf1ca0916efe2543a13a6da31e1083c61b08adc3602
    Tag UID: 672245080
    Upload status:
    Syncing 23 chunks       0s [==============================================================] 100 %
    Done! Your file is now retrievable from other Swarm nodes

In order to produce machine-readable output of the ``swarm up`` command, use the ``--no-track`` flag after the ``up`` keyword as follows:

.. code-block:: none
  
  $ echo "this is an example" > example.md
  $ swarm up --no-track example.md
    730c96f6de2b5b3b961b3cf1ca0916efe2543a13a6da31e1083c61b08adc3602

.. important:: The Swarm hash that was returned from the ``swarm up`` command does not infer your Swarm node has finished syncing the content to the network. You will have to wait until your content is synced until it is accessible from other Swarm nodes


HTTP
^^^^^^

The tag API can be accessed by HTTP using the ``GET`` verb on the ``bzz-tag:/`` locator.

``bzz-tag`` can track a tag by two parameters - either the Swarm hash or the tag UID of the upload.

You can find the tag UID of the upload in the data returned alongside the progress bar while using ``swarm up``. Tag retrieval by UID is meant for a future implementation that would allow tracking an upload that has not returned a Swarm hash yet; i.e. it is still being split and stored on the local node.

When uploading to Swarm via HTTP ``POST``, the tag UID will be returned as a header named ``x-swarm-tag``. This allows for programmatic access to tags for Dapp developers that would like to display upload progress to their users.

The tag associated with a Swarm hash can be retrieved with the hash inlined after ``bzz-tag:/`` while the tag associated with the tag UID can be retrieved using the UID as a query variable.

Using the Swarm Hash:


.. code-block:: none

   $ curl localhost:8500/bzz-tag:/730c96f6de2b5b3b961b3cf1ca0916efe2543a13a6da31e1083c61b08adc3602
   {
    "Uid": 12210768,
    "Name": "Some upload",
    "Address": "730c96f6de2b5b3b961b3cf1ca0916efe2543a13a6da31e1083c61b08adc3602",
    "Total": 2,
    "Split": 2,
    "Seen": 2,
    "Stored": 2,
    "Sent": 0,
    "Synced": 0,
    "StartedAt": "2019-09-30T12:20:11.176316707+05:30"
   }


Using the tag UID:

.. code-block:: none

   $ curl localhost:8500/bzz-tag:/&Id=12210768
   {
    "Uid": 12210768,
    "Name": "Some upload",
    "Address": "730c96f6de2b5b3b961b3cf1ca0916efe2543a13a6da31e1083c61b08adc3602",
    "Total": 2,
    "Split": 2,
    "Seen": 2,
    "Stored": 2,
    "Sent": 0,
    "Synced": 0,
    "StartedAt": "2019-09-30T12:20:11.176316707+05:30"
   }


