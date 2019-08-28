
Pinning Content
----

When content is uploaded in Swarm, it gets chunked and scattered over the network for storage. In the original Swarm design, the nodes that store the chunks gets incentivised for by the SWAP, SWEAR and SWINDLE protocols. For now, the incentivisation protocols are yet to be implemented. As a result of that, nodes does not have use preference in storing content. The default model used now is First Come First Serve (FIFO). The effect of the above model is that the uploaded contents will disappear after few days depending upon the overall network storage capacity. This is a undesirable property of the network today. To overcome this issue until the incentivisation layer is fully operational, pinning contents is implemented now. Aanyone can now pin a content (Swarm collection or a RAW file) on a Swarm node. i.e. a copy of the pinned content will be stored locally permenantly. Even if the content in the network disappears, it can be accessed from the pinned server always. 

.. note:: Pinned content will be available only from the Swarm node where it is pinned.


Pinning Content
^^^^^^^^^^^^^^^

Content can be pinned in two different ways. One is during the content upload and the other is thereafter.


1. Pinning during upload

   Add a header "x-swarm-pin" and set it to "true" when uploading content. This will upload the file and then pin it too.
   This method can be used for Tar, Multipart and RAW file uploads too.

.. code-block:: none

   curl -H "Content-Type: application/x-tar" -H "x-swarm-pin: true"  --data-binary @files.tar http://localhost:8500/bzz:/ 


2. Pinning after upload   

   If an already uploaded content needs to be pinned, the following HTTP API should be used.


.. code-block:: none

   # to pin a Swarm collection
   POST /bzz-pin:/<MANIFEST OR ENS NAME>

   # to pin a RAW file in Swarm
   POST /bzz-pin:/<SWARM RAW FILE HASH>/?raw=true 

.. note:: When pinning a already uploaded file, make sure that the entire file content is available locally by issuing a download once.


Unpinning Content
^^^^^^^^^^^^^^^^^

An already pinned file can be unpinned at anytime. Once the collection is unpinned, the contents will follow the FIFO rule and may be garbage collected in future.


.. code-block:: none

   DELETE /bzz-pin:/<MANIFEST OR ENS NAME OR SWARM RAW FILE HASH>



Listing Pinning Info
^^^^^^^^^^^^^^^^^^^^

Pinned contents and their information can be viewed at any point using this API. Information includes The pinned hash, wether the pinned content is a collection or RAW file, the pinned content size in bytes and the no of time the content is pinned.


.. code-block:: none

   GET /bzz-pin:/

   [
    { "Address"    : "0x94f78a45c7897957809544aa6d68aa7ad35df695713895953b885aca274bd955",
      "IsRaw"      : "false",
      "FileSize"   : "12046",
      "PinCounter" : "2",
    },  
    { "Address"    : "0xccef599d1a13bed9989e424011aed2c023fce25917864cd7de38a761567410b8",
      "IsRaw"      : "true",
      "FileSize"   : "146",
      "PinCounter" : "5",
    },
   ]

.. note:: The information will be returned in json format shown above   
