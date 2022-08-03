Article metadata
================

The articles metadata is aligned on the [TZIP-021 specification](https://tzip.tezosagora.org/proposal/tzip-21/) which is used to define assets in the Tezos ecosystem.

We define the following fields, as part of the specification:

* `decimals`
* `symbol`: ARTKL
* `name`: title of the article
* `description`: abstract of the article 
* `minter`
* `type`: "article" 
* `tags`
* `language`
* `artifactUri`: ipfs uri pointing to a [fx-markdown](./fx-markdown.md) text file
* `displayUri`: HQ version of the thumbnail
* `thumbnailUri`: LQ version of the thumbnail

2 extra fields were added:

* `platforms`: *(optional)* **(array)** a list of **string** defining the platforms for which the content was written
* `thumbnailCaption`: *(optional)* **(string)** a caption for the thumbnail


## Platforms

We think about having articles being used for the whole ecosystem. As such, we can imagine a single front-end where we could find any article about the tezos ecosystem, providing an extensive tooling such as a well designed editor, first-class integration of tezos nfts... etc. However, we would like to facilitate the integration of the minted articles into existing applications without the need of including articles from the whole ecosystem.

As such, we propose the `platforms` field, a list of platforms picked by the writer when minting the article, specifying in which platform(s) their article could be integrated.

It would be up to the platforms to decide whether or not they want to include an article based on its target platforms.

This is a **non-exhaustive** and quick list of existing platforms, feel free to make a PR to add a platform to the list:

* kalamint
* hicetnunc
* akaswap
* objkt
* fxhash
* tz1and
* versum
* teia
* 8bidou
* typed


## Example

```json
{
  "decimals": 0,
  "symbol": "ARTKL",
  "name": "An all-medias article",
  "description": "This is an article with all the blocks to check the make sure the design of every block in the final article render works properly",
  "minter": "tz1MGzgRu6qJ3RaBUErnpFDLarFVPgaApKrS",
  "type": "article",
  "tags": [],
  "language": "en-US",
  "artifactUri": "ipfs://Qmbn9nuSfix1LXhoNi9YjUsA519jA39VWiNUuV4ioyAm4c",
  "displayUri": "ipfs://QmZGNvvPKd6UxfHzUseEbGty9HRbT41aT5sj1ep572vWv4",
  "thumbnailUri": "ipfs://QmP6wGY6xA969id4Va7GaH6SV8UFpCYpZ4BTMmJU6w4feg",
  "platforms": [
    "fxhash"
  ],
  "thumbnailCaption": "Some random caption"
}
```