---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 📄 Signing

***

### Main concepts

Assets, events and documents are main components of ZIMT Hub system.\
\
Object data inside all of them is immutable data, as once hub receives that data from a client, it is hashed (<mark style="color:blue;">`object_hash`</mark>) and put inside <mark style="color:orange;">`receipt`</mark> object along side other details, like, hub's address (<mark style="color:blue;">`received_by`</mark>), as a receiver of the data, <mark style="color:orange;">`organization`</mark> resource belongs to and the timestamp (<mark style="color:blue;">`received_at`</mark>) of reception in milliseconds.

#### ID

Receipt object is hashed, and that hash represents a unique `id` of that object.

#### Proof

Unique ID string, is signed with hub's private key, which represents `proof` of that object.

```json
{
   "proof":"0x0192cbd1c59b40ea97f7bc102a16c325a3066bb6b68c9c16bae447d8bb38565a66da29...",
   "id":"0x1512258c1a082a1148e655cf4abf13b914e31e7e485191c2b6b5ee466e03c951",
   "receipt":{
      "object_hash":"0xc0d7efb7eaa769f83a8ce2d41466d603af6ad308b5a8053676c4034d0369aec5",
      "received_by":"0x678b3c5090B25b3a63120CF0218750886e37A96E",
      "received_at":1579278115,
      "organization":"0x123..."
   }
}
```

***

#### Bundle (disabled)

Hub periodically creates bundles, where one of the bundles will contain object's proof string.

Bundle then hashes its array of proofs into `entries_hash` property of a `meta` object, that also contains, hub address `created_by`, `created_at` timestamp, and `strategy` id of the strategy used for bundling.

That meta object is then hashed into a unique `id` string, and same as above, bundle `id` string is then signed with hub's private key to generate unique bundle `proof` string. And finally, bundle unique proof string, is stored in a blockchain.

Bundle, once put into a transaction in a blockchain, will contain blockchain `url` and transaction hash `tx_hash`.

***

#### Proving data integrity

Having read concepts above, you can easily prove if data has/has not changed since it was initially created, in few steps:

1. Get bundle that bundled the proof of an object you want to prove data integrity for.\\
2. Based on transaction hash, get the transaction from the blockchain.\\
3. Inside that transaction is bundle proof string.\
   \
   Having now bundle content (that contains object's proof string in `entries` array), you can do all the same process hashing of entries to get entries\_hash, then hashing meta to get the bundle id, and having hub's public key (`address`), using <mark style="color:blue;">ethers.js</mark> extracting (signed content (`id`)) from bundles proof string.\
   \
   If proof string is the same in fetched bundle and transaction, and id extracted from proof string is the same as bundle id you recalculated, proves that bundle hasn't changed since it was initially created.\\
4. Now that we proved bundle's data integrity, we have to do similar process to prove that object's proof contained in the bundle is valid.\\
5. Get the object (`asset`, `event`, `document`, etc.) from hub, and proceed with the same process as above, hashing the object to get `object_hash`, hashing receipt object, to get the object's id, and now having recalculated object's id and hub's public key (`address`), using again <mark style="color:blue;">ethers.js</mark> method we can extract id that was initially signed from the proof string of the object contained in the bundle we just confirmed integrity for.\
   \
   If obect `proof` is same as object's proof string in the bundle, and extracted id from the proof string is the same as id we just recalculated, proves object hasn't change since it was initially created.
