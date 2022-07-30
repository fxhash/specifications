Tezos Storage Pointers
======================

A Tezos Storage Pointer is a declarative list of key-value properties pointing to some content stored in a Tezos Smart Contract. Primarly designed to point to NFTs, this spec can also be used to point to any data. This document provides an implementation-agnostic specification.

A **Tezos Smart Pointer** is a list of key-value `string` pairs.

| key | description | value format | required | default | example |
| --- | ----------- | ------------ | -------- | ------- | ------- |
| contract | The address of a Smart Contract | a string starting with KT1 | yes | | `KT1BJC12dG17CVvPKJ1VYaNnaT5mzfnUTwXv` |
| path | The path to the data from the root of a contract | A list of dot-separated values, where each value points to a lower node in the exploration of the tree | yes | | `token_metadata::1452` where `token_metadata` is the bigmap annotation name in the storage and `1452` is the key of the data in the bigmap |
|


Why 2 properties are used to point to the data (`path` & `value_path`). 