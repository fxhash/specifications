Tezos Storage Pointers
======================

A Tezos Storage Pointer is a declarative list of key-value properties pointing to some content stored in a Tezos Smart Contract. Primarly designed to point to NFTs, this spec can also be used to point to any data. This document provides an implementation-agnostic specification.


# Specification

A **Tezos Storage Pointer** is a list of key-value `string` pairs.

## contract

Location of the contract pointed to.

* **key**: `contract`
* **value format**: `<address>.<network>`
  * Valid addresses are base58check-encoded Tezos contract addresses: `KT1....`
  * Valid networks are base58check-encoded Tezos chain identifiers `Net....` or known public network aliases: `mainnet`, `carthagenet`, `delphinet`, …
  * `<network>` is optionnal, if undefined the "current" network in a given context will be picked
* **required**: yes
* **default**: /
* **example**: `KT1BJC12dG17CVvPKJ1VYaNnaT5mzfnUTwXv` or `KT1BJC12dG17CVvPKJ1VYaNnaT5mzfnUTwXv.mainnet`


## path

The path to the data to fetch from the root of the contract.

* **key**: `path`
* **value format**: A list of 2-colon-separated values, where each value points to a lower node in the exploration of the tree. Values can be hierachical (for instance: `assets.ledger`). If a value represents a key to traverse the tree further down, it can be a plain value `abcde...` or complex (an object or an array), you can specify it as is, for example, `{'address':'tz123','nat':'123'}`. Double quotes `"` are not accepted.
* **required**: yes
* **default**: /
* **example**: `token_metadata::1452`, `assets.ledger::774`, `ledger::{'address':'tz123','nat':'123'}`

## value_path

The path to the data from the root of the data fetched using `path`. This property is used to facilitate the retrieval of the target data, as in many cases there needs to be a 2-step fetch: first getting the data from the contract and then processing the data to extract what's targetted.

* **key**: `value_path`
* **value format**: Same a `path`
* **required**: no
* **default**: / (get the whole data fetched from the contract storage)
* **example**: `token_info::`, `map_key::map_key2::12::age`

## storage_type

The data type of the element at the root of the storage, ie: the type of the element of the first data pointed by the first value of the `path`.

* **key**: `storage_type`
* **value format**: string
* **required**: no
* **default**: `bigmap`
* **example**:

## data_spec

The specification of the data to retrieve.

* **key**: `data_spec`
* **value format**: string
* **required**: no
* **default**: `TZIP-021` (FA2 asset)
* **example**: `TZIP-021`


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
data_spec: ""
```

## A Generative Token on fxhash

```
contract: "KT1Sy7X6TubmZ39G8CHVrUcxjc3jiF68P8oB"
path: "ledger::85"
data_spec: "FX-GEN-DATA-002"
```