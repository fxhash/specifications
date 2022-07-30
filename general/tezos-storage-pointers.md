Tezos Storage Pointers
======================

A Tezos Storage Pointer is a declarative list of key-value properties pointing to some content stored in a Tezos Smart Contract. Primarly designed to point to NFTs, this spec can also be used to point to any data. This document provides an implementation-agnostic specification.


# Specification

A **Tezos Storage Pointer** is a list of key-value `string` pairs.

| key | description | value format | required | default | example |
| --- | ----------- | ------------ | -------- | ------- | ------- |
| contract | The address of a Smart Contract | a string starting with KT1 | yes | | `KT1BJC12dG17CVvPKJ1VYaNnaT5mzfnUTwXv` |
| path | The path to the data to fetch from the root of a contract | A list of 2-colon-separated values, where each value points to a lower node in the exploration of the tree | yes | | `token_metadata::1452` where `token_metadata` is the bigmap annotation name in the storage and `1452` is the key in the bigmap |
| value_path | The path to the data from the root of the data fetched using `path` | A list of 2-colon-separated values, where each value points to a lower node in the exploration of the tree. If empty string, the whole data is retrieved from `path`. | no | `token_info::` | `token_info::` to retrieve the metadata URI from the data fetched. There are 2 values: `'token_info'` and `''` (empty string), which means that when data is fetched from path, then we lookup `'token_info'`->`''` in the object retrieved |
| storage_type | The data type of the storage in which we want to retrieve the data. ie: the type of the first value in the path | string | no | `bigmap` | |
| spec | The specification of the contract. | string | no | `TZIP-012` | `TZIP-012` (FA2 contract) |
| data_spec | The specification of the data to retrieve. | no | string | `TZIP-021` | `TZIP-021` (FA2 asset) |


## The need for 2 paths properties: `path` and `value_path`

Most general-purpose indexers (such as tzkt) have generic endpoints to retrieve data from the blockchain. By splitting the path to the actual data of interest, we facilitate its retrieval which happens in a 2-step process:

* query the indexer with the `path` to get the value stored
* use `value_path` to lookup the data of interest from the data returned at the previous step

`path` and `value_path` can be concatenated to a single pointer, however having these separated will facilitate the usage of this spec for fetching data in practice.


# Examples

## A Hic et Nunc token

```
contract: "KT1RJ6PbjHpwc3M5rw5s2Nbmefwbuwbdxton"
path: "token_metadata::763663"
```

## A fxhash token

```
contract: "KT1U6EHmNxJTkvaWJ4ThczG4FSDaHC21ssvi"
path: "token_metadata::1045062"
```

## An objkt marketplace 2.0 listing

```
contract: "KT1WvzYHCNBvDSdwafTHv7nJ1dWmZ8GCYuuC"
path: "asks::1903555"
value_path: ""
spec: ""
data_spec: ""
```

## A Generative Token on fxhash

```
contract: "KT1Sy7X6TubmZ39G8CHVrUcxjc3jiF68P8oB"
path: "ledger::85"
value_path: ""
spec: "FX-ISSUER-002"
data_spec: "FX-GEN-DATA-002"
```