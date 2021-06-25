# IPFS

## Content Addressing

### What is a CID?

- A particular form of Content Addressing for the Decentralized Web.
- It was developed for IPFS, but has broad implications.
- It contains Cryptographic Hash and a Codec.
- Codecs encode and decode data in certain formats.
- Git, Bitcoin and Ethereum all use content addressing.
- CID creates an universal identifier for any of these systems.

```zsh
+-------+------------------------------+
| Codec | Multihash                    |
+-------+------------------------------+
```

- Multihash or the self-describing hash, contains Hash Type and Hash Value.

```zsh
+------------------------------+
| Codec                        |
+------------------------------+
|                              |
| Multihash                    |
| +----------+---------------+ |
| |Hash Type | Hash Value    | |
| +----------+---------------+ |
|                              |
+------------------------------+
```

- We generate fingerprint by using a cryptogrphic algorithm that maps input of arbitrary size (data or a file) to output of fixed size.
- We call the output cryptographic hash digest or simply hash.

![Fingerprint](https://proto.school/tutorial-assets/T0006L01-crypto-algo-256.png)

The cryptographic algorithm used must generate hashes that have the following characteristics:

- Deterministic: The same input should always produce the same hash.
- Uncorrelated: A small change in the input should generate a completely different hash.
- One-way: It should be infeasible to reconstruct the data from the hash.
- Unique: Only one file can produce one specific hash.

IPFS uses `sha2-256` by default, though a CID supports any strong cryptographic hash algorithm.

### Multihash

- Hashing algorithms may prove to be insecure through time, so multihash adds the ability to know which algorithms was used to generate the hash, thus making the CID generalized.
- For this reason it is called the self-describing hash.
- Multihash follow the `TLV` pattern (`type-length-value`).

![Multihash Format](https://proto.school/tutorial-assets/T0006L02-multihash.jpg)

- `type`: identifier of the cryptographic algorithm used to generate the hash (e.g. the identifier of `sha2-256` would be `18` - `0x12` in hexadecimal) - see the [multicodec table](https://github.com/multiformats/multicodec/blob/master/table.csv) for all the identifiers
- `length`: the actual length of the hash (using `sha2-256` it would be `256` bits, which equates to `32` bytes)
- `value`: the actual hash value

- To display `0` and `1` of the output to string, IPFS uses `base58btc` encoding. This version was known as `CID0`.
- The initials `Qm...` is thus common in IPFS.

Problems with `CID0`

- How do we know what method was used to encode the data?
- How do we know what method was used to create the string representation of the CID? Will we always be using `base58btc`?

These problems were addressed in `CID1`, which is the current version of CID.

### `CID1` : Multicodec Prefix

- This new version adds a new prefix known as Multicodec Prefix which tells which encoding was used on the data.
- It could tell whether it is encoded with CBOR, Protobuf, plain JSON, etc.
- Here's the [complete table](https://github.com/multiformats/multicodec/blob/master/table.csv).

![CID01](https://proto.school/tutorial-assets/T0006L03-multicodec.png)

- The `dag-pb` is one of many different types of [IPLD](https://ipld.io/) (InterPlanetary Linked Data) codecs.
- This multicodec is not only limited to IPFS and IPLD but is a part of the [Multformats](https://multiformats.io/) project.
- Multiformats project has implications in wide variety of projects including the CID.
