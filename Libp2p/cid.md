# Self-Describing Content-Addressed Identifiers

Small identifiers would prefer a compact version of multicodec. This compact identifier should be standardized and stored in in a look up table as a single variant.

Because IPLD fosters multiple different formats, it is necessary to formalize a process that facilitates identification of content type while traversing an IPLD link.

The final solution proposed by [CIDv1](https://github.com/ipfs/specs/issues/130) entails changing how IPLD links are represented by 
1. teaching IPLD links to carry data formats (using multicodec)
2. teaching IPLD links to distinguish base encodings

## (2) Add Multibase Prefix to Representations of IPLD Links

## (2) IPLD Links learn about Codecs

* introduce a multicodec-packed variant prefix to the link `=>` signals encoding of the linked-to object
* link to the object carries information regarding its interpretation

All proper IPLD formats should carry the multicodec header at the beginning of the serialized representation. This enables client-side authentication without even having a link.

## CIDv1
> [CIDv1](https://github.com/ipfs/specs/issues/130)

A multibase prefix (`mhash`) to describe the base that encodes the CID.
A version number (`version`) for the CID.
A packed multicodec (`mcodec`) for the CID multicodec table/
A multihash (`mhash`), including: `<mhash-code><mhash-len><mhash-value>`

```
<mbase><version><mcodec><mhash>
```

### References

* [multiformats/cid](https://github.com/multiformats/cid)
* [multiformats/rust-cid](https://github.com/multiformats/rust-cid)