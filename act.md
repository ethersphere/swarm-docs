Access control

* encryption hierarchical share subdirectory

root access - requires disclosed reference

Is necessary, but might not be sufficient for any other type of access.

granted access - requires root access and authorized private key

Granted access allows multiple parties sharing the same root access
differentiated privileges accessing the content.

Granted access is implemented by a key-value map in manifest format called
ACT (access control trie) where the key is the hash of the D-H secret of the
publisher and grantee, the value is an access key encrypted with the public key
of the grantee. The actual reference is encrypted with this access key.

This allows for updating the content without changing the ACT, by encrypting
the new reference with the same access key. The access key has the same scope
as the ACT. The ACT itself can be either unencrypted or entirely accessible
given root access. The same ACT can be used for multiple files and can be
referenced both by hard and symbolic references (mutable resource references or
ENS names).

Checking and looking up ones own access is logarithmic in the size of the ACT.
The size of the ACT merely provides an upper bound on the number of grantees,
but does not disclose any information beyond this upper bound about the grantee
set to third parties. Even those included in ACT can only learn that they are
grantees, but obtain no information about other grantees beyond an upper bound
on their number.

Granting access to an additional key requires extending the ACT by a single
entry, which is logarithmic in the size of the ACT. Withdrawing access
requires a change in the access key and therefore the rebuilding of the ACT.
Note that this requires that the publisher has the public keys of grantees.

In the simplest case, the access key is a symmetric encryption/decryption
key. However, this is just a special case of the more flexible solution, where
the access key consists of a symmetric key and a key derivation path by which
it is derived from a root key. In this case, in addition to the encrypted
reference, a derivation path may also be included. Any party with an
access key whose derivation is a prefix to the derivation path of the
reference can decrypt the reference by deriving its key using their own key
and the rest of the derivation path of the reference.

This allows for a tree-like hierarchy of roles, possibly reflecting an
organizational structure. As long as role changes are "promotions", i.e.
result in increase of privileges, it is sufficient to change a single
ACT entry for each role change.

* access control survives content update
* access is easily checked and not
