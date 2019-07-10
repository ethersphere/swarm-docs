Uploading and downloading
=======================

..  contents::

Introduction
==================================
.. note:: This guide assumes you've installed the Swarm client and have a running node that listens by default on port 8500. See `Getting Started <./gettingstarted.html>`_ for details.

Arguably, uploading and downloading content is the raison d'être of Swarm. Uploading content consists of "uploading" content to your local Swarm node, followed by your local Swarm node "syncing" the resulting chunks of data with its peers in the network. Meanwhile, downloading content consists of your local Swarm node querying its peers in the network for the relevant chunks of data and then reassembling the content locally.

Uploading and downloading data can be done through the ``swarm`` command line interface (CLI) on the terminal or via the HTTP interface on `http://localhost:8500 <http://localhost:8500>`_.

Using HTTP
======================

Swarm offers an HTTP API. Thus, a simple way to upload and download files to/from Swarm is through this API.
We can use the ``curl`` `tool <https://curl.haxx.se/docs/httpscripting.html>`_ to exemplify how to interact with this API.

.. note:: Files can be uploaded in a single HTTP request, where the body is either a single file to store, a tar stream (application/x-tar) or a multipart form (multipart/form-data).

To upload a single file to your node, run this:

.. code-block:: none

  $ curl -H "Content-Type: text/plain" --data "some-data" http://localhost:8500/bzz:/

Once the file is uploaded, you will receive a hex string which will look similar to this:

.. code-block:: none

  027e57bcbae76c4b6a1c5ce589be41232498f1af86e1b1a2fc2bdffd740e9b39

This is the Swarm hash of the address string of your content inside Swarm. It is the same hash that would have been returned by using the :ref:``swarm up <swarmup>`` command.

To download a file from Swarm, you just need the file's Swarm hash. Once you have it, the process is simple. Run:

.. code-block:: none

  $ curl http://localhost:8500/bzz:/027e57bcbae76c4b6a1c5ce589be41232498f1af86e1b1a2fc2bdffd740e9b39/

The result should be your file:

.. code-block:: none

  some-data

And that's it.

.. note:: If you omit the trailing slash from the url then the request will result in a HTTP redirect. The semantically correct way to access the root path of a Swarm manifest is using the trailing slash.

Tar stream upload
------------------

Tar is a traditional unix/linux file format for packing a directory structure into a single file. Swarm provides a convenient way of using this format to make it possible to perform recursive uploads using the HTTP API.

.. code-block:: none

  # create two directories with a file in each
  $ mkdir dir1 dir2
  $ echo "some-data" > dir1/file.txt
  $ echo "some-data" > dir2/file.txt

  # create a tar archive containing the two directories (this will tar everything in the working directory)
  tar cf files.tar .

  # upload the tar archive to Swarm to create a manifest
  $ curl -H "Content-Type: application/x-tar" --data-binary @files.tar http://localhost:8500/bzz:/
  > 1e0e21894d731271e50ea2cecf60801fdc8d0b23ae33b9e808e5789346e3355e

You can then download the files using:

.. code-block:: none

  $ curl http://localhost:8500/bzz:/1e0e21894d731271e50ea2cecf60801fdc8d0b23ae33b9e808e5789346e3355e/dir1/file.txt
  > some-data

  $ curl http://localhost:8500/bzz:/1e0e21894d731271e50ea2cecf60801fdc8d0b23ae33b9e808e5789346e3355e/dir2/file.txt
  > some-data

GET requests work the same as before with the added ability to download multiple files by setting `Accept: application/x-tar`:

.. code-block:: none

  $ curl -s -H "Accept: application/x-tar" http://localhost:8500/bzz:/ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8/ | tar t
  > dir1/file.txt
    dir2/file.txt


Multipart form upload
---------------------

.. code-block:: none

  $ curl -F 'dir1/file.txt=some-data;type=text/plain' -F 'dir2/file.txt=some-data;type=text/plain' http://localhost:8500/bzz:/
  > 9557bc9bb38d60368f5f07aae289337fcc23b4a03b12bb40a0e3e0689f76c177

  $ curl http://localhost:8500/bzz:/9557bc9bb38d60368f5f07aae289337fcc23b4a03b12bb40a0e3e0689f76c177/dir1/file.txt
  > some-data

  $ curl http://localhost:8500/bzz:/9557bc9bb38d60368f5f07aae289337fcc23b4a03b12bb40a0e3e0689f76c177/dir2/file.txt
  > some-data


Add files to an existing manifest using multipart form
------------------------------------------------------

.. code-block:: none

  $ curl -F 'dir3/file.txt=some-other-data;type=text/plain' http://localhost:8500/bzz:/9557bc9bb38d60368f5f07aae289337fcc23b4a03b12bb40a0e3e0689f76c177
  > ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8

  $ curl http://localhost:8500/bzz:/ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8/dir1/file.txt
  > some-data

  $ curl http://localhost:8500/bzz:/ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8/dir3/file.txt
  > some-other-data


Upload files using a simple HTML form
-------------------------------------

.. code-block:: html

  <form method="POST" action="/bzz:/" enctype="multipart/form-data">
    <input type="file" name="dir1/file.txt">
    <input type="file" name="dir2/file.txt">
    <input type="submit" value="upload">
  </form>


Listing files
-------------

.. note:: The ``jq`` command mentioned below is a separate application that can be used to pretty-print the json data retrieved from the ``curl`` request

A `GET` request with ``bzz-list`` URL scheme returns a list of files contained under the path, grouped into common prefixes which represent directories:

.. code-block:: none

   $ curl -s http://localhost:8500/bzz-list:/ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8/ | jq .
   > {
      "common_prefixes": [
        "dir1/",
        "dir2/",
        "dir3/"
      ]
    }

.. code-block:: none

    $ curl -s http://localhost:8500/bzz-list:/ccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8/dir1/ | jq .
    > {
      "entries": [
        {
          "path": "dir1/file.txt",
          "contentType": "text/plain",
          "size": 9,
          "mod_time": "2017-03-12T15:19:55.112597383Z",
          "hash": "94f78a45c7897957809544aa6d68aa7ad35df695713895953b885aca274bd955"
        }
      ]
    }

Setting ``Accept: text/html`` returns the list as a browsable HTML document.