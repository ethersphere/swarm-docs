CLI - Command Line Interface
============================

.. _swarmup:

Uploading a file to your local Swarm node
------------------------------------------

.. note:: Once a file is uploaded to your local Swarm node, your node will `sync` the chunks of data with other nodes on the network. Thus, the file will eventually be available on the network even when your original node goes offline.

The basic command for uploading to your local node is ``swarm up FILE``. For example, let's create a file called example.md and issue the following command to upload the file example.md file to your local Swarm node.

.. code-block:: none
  
  $ echo "this is an example" > example.md
  $ swarm up example.md
  > d1f25a870a7bb7e5d526a7623338e4e9b8399e76df8b634020d11d969594f24a

The hash returned is the hash of a :ref:`swarm manifest <swarm-manifest>`. This manifest is a JSON file that contains the ``example.md`` file as its only entry. Both the primary content and the manifest are uploaded by default.

After uploading, you can access this example.md file from Swarm by pointing your browser to:

.. code-block:: none

  $ http://localhost:8500/bzz:/d1f25a870a7bb7e5d526a7623338e4e9b8399e76df8b634020d11d969594f24a/

The manifest makes sure you could retrieve the file with the correct MIME type.

You can encrypt your file using the ``--encrypt`` flag. See the :ref:`Encryption` section for details.


Suppressing automatic manifest creation
---------------------------------------
You may wish to prevent a manifest from being created alongside with your content and only upload the raw content. You might want to include it in a custom index, or handle it as a data-blob known and used only by a certain application that knows its MIME type. For this you can set ``--manifest=false``:

.. code-block:: none

  $ swarm --manifest=false up FILE
  > 7149075b7f485411e5cc7bb2d9b7c86b3f9f80fb16a3ba84f5dc6654ac3f8ceb

This option suppresses automatic manifest upload. It uploads the content as-is.
However, if you wish to retrieve this file, the browser can not be told unambiguously what that file represents.
In the context, the hash ``7149075b7f485411e5cc7bb2d9b7c86b3f9f80fb16a3ba84f5dc6654ac3f8ceb`` does not refer to a manifest. Therefore, any attempt to retrieve it using the ``bzz:/`` scheme will result in a ``404 Not Found`` error. In order to access this file, you would have to use the :ref:`bzz-raw` scheme.


Downloading a single file
----------------------------

To download single files, use the ``swarm down`` command.
Single files can be downloaded in the following different manners. The following examples assume ``<hash>`` resolves into a single-file manifest:

.. code-block:: none

  $ swarm down bzz:/<hash>            #downloads the file at <hash> to the current working directory
  $ swarm down bzz:/<hash> file.tmp   #downloads the file at <hash> as ``file.tmp`` in the current working dir
  $ swarm down bzz:/<hash> dir1/      #downloads the file at <hash> to ``dir1/``

You can also specify a custom proxy with `--bzzapi`:

.. code-block:: none

  $ swarm --bzzapi http://localhost:8500 down bzz:/<hash>            #downloads the file at <hash> to the current working directory using the localhost node


Downloading a single file from a multi-entry manifest can be done with (``<hash>`` resolves into a multi-entry manifest):

.. code-block:: none

  $ swarm down bzz:/<hash>/index.html            #downloads index.html to the current working directory
  $ swarm down bzz:/<hash>/index.html file.tmp   #downloads index.html as file.tmp in the current working directory
  $ swarm down bzz:/<hash>/index.html dir1/      #downloads index.html to dir1/

..If you try to download from a multi-entry manifest without specifying the file, you will get a `got too many matches for this path` error. You will need to specify a `--recursive` flag (see below).

Uploading to a remote Swarm node
-----------------------------------
You can upload to a remote Swarm node using the ``--bzzapi`` flag.
For example, you can use one of the public gateways as a proxy, in which case you can upload to Swarm without even running a node.


.. code-block:: none

  $ swarm --bzzapi https://swarm-gateways.net up example.md

.. note:: This gateway currently only accepts uploads of limited size. In future, the ability to upload to this gateways is likely to disappear entirely.


Uploading a directory
---------------------

Uploading directories is achieved with the ``--recursive`` flag.

.. code-block:: none

  $ swarm --recursive up /path/to/directory
  > ab90f84c912915c2a300a94ec5bef6fc0747d1fbaf86d769b3eed1c836733a30

The returned hash refers to a root manifest referencing all the files in the directory.

Directory with default entry
----------------------------

It is possible to declare a default entry in a manifest. In the example above, if ``index.html`` is declared as the default, then a request for a resource with an empty path will show the contents of the file ``/index.html``

.. code-block:: none

  $ swarm --defaultpath /path/to/directory/index.html --recursive up /path/to/directory
  > ef6fc0747d1fbaf86d769b3eed1c836733a30ab90f84c912915c2a300a94ec5b

You can now access index.html at

.. code-block:: none

  $ http://localhost:8500/bzz:/ef6fc0747d1fbaf86d769b3eed1c836733a30ab90f84c912915c2a300a94ec5b/

and also at

.. code-block:: none

  $ http://localhost:8500/bzz:/ef6fc0747d1fbaf86d769b3eed1c836733a30ab90f84c912915c2a300a94ec5b/index.html

This is especially useful when the hash (in this case ``ef6fc0747d1fbaf86d769b3eed1c836733a30ab90f84c912915c2a300a94ec5b``) is given a registered name like ``mysite.eth`` in the `Ethereum Name Service <./ens.html>`_. In this case the lookup would be even simpler:

.. code-block:: none

  http://localhost:8500/bzz:/mysite.eth/

.. note:: You can toggle automatic default entry detection with the ``SWARM_AUTO_DEFAULTPATH`` environment variable. You can do so by a simple ``$ export SWARM_AUTO_DEFAULTPATH=true``. This will tell Swarm to automatically look for ``<uploaded directory>/index.html`` file and set it as the default manifest entry (in the case it exists).  

Downloading a directory
--------------------------

To download a directory, use the ``swarm down --recursive`` command.
Directories can be downloaded in the following different manners. The following examples assume <hash> resolves into a multi-entry manifest:

.. code-block:: none

  $ swarm down --recursive bzz:/<hash>            #downloads the directory at <hash> to the current working directory
  $ swarm down --recursive bzz:/<hash> dir1/      #downloads the file at <hash> to dir1/

Similarly as with a single file, you can also specify a custom proxy with ``--bzzapi``:

.. code-block:: none

  $ swarm --bzzapi http://localhost:8500 down --recursive bzz:/<hash> #note the flag ordering

.. important :: Watch out for the order of arguments in directory upload/download: it's ``swarm --recursive up`` and ``swarm down --recursive``.

Adding entries to a manifest
-------------------------------
The command for modifying manifests is ``swarm manifest``.

To add an entry to a manifest, use the command:

.. code-block:: none

  $ swarm manifest add <manifest-hash> <path> <hash> [content-type]

To remove an entry from a manifest, use the command:

.. code-block:: none

  $ swarm manifest remove <manifest-hash> <path>

To modify the hash of an entry in a manifest, use the command:

.. code-block:: none

  $ swarm manifest update <manifest-hash> <path> <new-hash>

Reference table
-----------------

+------------------------------------------+------------------------------------------------------------------------+
| **upload**                               | ``swarm up <file>``                                                    |
+------------------------------------------+------------------------------------------------------------------------+
| ~ dir                                    | ``swarm --recursive up <dir>``                                         |
+------------------------------------------+------------------------------------------------------------------------+
| ~ dir w/ default entry (here: index.html)| ``swarm --defaultpath <dir>/index.html --recursive up <dir>``          |
+------------------------------------------+------------------------------------------------------------------------+ 
| ~ w/o manifest                           | ``swarm --manifest=false up``                                          |
+------------------------------------------+------------------------------------------------------------------------+
| ~ to remote node                         | ``swarm --bzzapi https://swarm-gateways.net up``                       |
+------------------------------------------+------------------------------------------------------------------------+
| ~ with encryption                        | ``swarm up --encrypt``                                                 |
+------------------------------------------+------------------------------------------------------------------------+
| **download**                             | ``swarm down bzz:/<hash>``                                             |
+------------------------------------------+------------------------------------------------------------------------+
| ~ dir                                    | ``swarm down --recursive bzz:/<hash>``                                 |
+------------------------------------------+------------------------------------------------------------------------+
| ~ as file                                | ``swarm down bzz:/<hash> file.tmp``                                    |
+------------------------------------------+------------------------------------------------------------------------+
| ~ into dir                               | ``swarm down bzz:/<hash> dir/``                                        |
+------------------------------------------+------------------------------------------------------------------------+
| ~ w/ custom proxy                        | ``swarm down --bzzapi http://<proxy address> down bzz:/<hash>``        |
+------------------------------------------+------------------------------------------------------------------------+
| **manifest**                             |                                                                        |
+------------------------------------------+------------------------------------------------------------------------+
| add ~                                    | ``swarm manifest add <manifest-hash> <path> <hash> [content-type]``    |
+------------------------------------------+------------------------------------------------------------------------+
| remove ~                                 | ``swarm manifest remove <manifest-hash> <path>``                       |
+------------------------------------------+------------------------------------------------------------------------+
| update ~                                 | ``swarm manifest update <manifest-hash> <path> <new-hash>``            |
+------------------------------------------+------------------------------------------------------------------------+

Up- and downloading in the CLI: example usage
----------------------------------

.. tabs::

  .. group-tab:: Up/downloading

    Let's create a dummy file and upload it to Swarm:

    .. code-block:: none

      $ echo "this is a test" > myfile.md
      $ swarm up myfile.md
      > <reference hash>

    We can download it using the ``bzz:/`` scheme and give it a name.

    .. code-block:: none

      $ swarm down bzz:/<reference hash> iwantmyfileback.md
      $ cat iwantmyfileback.md
      > this is a test

    We can also ``curl`` it using the HTTP API.

    .. code-block:: none

      $ curl http://localhost:8500/bzz:/<reference hash>/
      > this is a test

    We can use the ``bzz-raw`` scheme to see the manifest of the upload.

    .. code-block:: none

      $ curl http://localhost:8500/bzz-raw:/<reference hash>/

    This returns the manifest:

    .. code-block:: none

      {
        "entries": [
          {
            "hash": "<file hash>",
            "path": "myfile.md",
            "contentType": "text/markdown; charset=utf-8",
            "mode": 420,
            "size": 15,
            "mod_time": "<timestamp>"
          }
        ]
      }

  .. group-tab:: Up/down as is

    We can upload the file as-is:

    .. code-block:: none

      $ echo "this is a test" > myfile.md
      $ swarm --manifest=false up myfile.md
      > <as-is reference hash>

    We can retrieve it using the ``bzz-raw`` scheme in the HTTP API.

    .. code-block:: none

      $ curl http://localhost:8500/bzz-raw:/<as-is reference hash>/
      > this is a test

  .. group-tab:: Manipulate manifests

    Let's create a directory with a dummy file, and upload the directory to swarm.

    .. code-block:: none 

      $ mkdir dir
      $ echo "this is a test" > dir/dummyfile.md
      $ swarm --recursive up dir
      > <dir hash>

    We can look at the manifest using ``bzz-raw`` and the HTTP API.

    .. code-block:: none 
    
      $ curl http://localhost:8500/bzz-raw:/<dir hash>/

    It will look something like this:

    .. code-block:: none

      {
        "entries": [
          {
            "hash": "<file hash>",
            "path": "dummyfile.md",
            "contentType": "text/markdown; charset=utf-8",
            "mode": 420,
            "size": 15,
            "mod_time": "2018-11-11T16:52:07+01:00"
          }
        ]
      }

    We can remove the file from the manifest using ``manifest remove``.

    .. code-block:: none

      $ swarm manifest remove <dir hash> "dummyfile.md"
      > <new dir hash>

    When we check the new dir hash, we notice that it's empty -- as it should be.

    Let's put the file back in there.

    .. code-block:: none

      $ swarm up dir/dummyfile.md
      > <individual file hash>
      $ swarm manifest add <new dir hash> "dummyfileagain.md" <individual file hash>
      > <new dir hash 2>

    We can check the manifest under <new dir hash 2> to see that the file is back there.