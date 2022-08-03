fx-markdown
===========

The fx-markdown specification is an extension of the markdown specification based on another markdown extension, [Github Flavored Markdown](https://github.github.com/gfm/).


# Github Flavored Markdown

We base our specification on the Github Flavored Markdown because we believe it provides a necessary set of additionnal features which make sense in the context of write ups about generative art. As it is now a common and widely used spec, it is integrated by many parsers (most often as a plugin).

The GFM spec mainly introduces the following element:

* tables
* strikethrough
* auto links

See the [spec](https://github.github.com/gfm/) for more details.


# Math expressions

Github decided to introduce math expressions in their markdown spec (not a part of GFM) so we decided to align on their spec for including math formulas in the articles. 

See [Writing mathematical expressions, Github doc](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions).

Summary:
* math expressions in TeX
* inline: expression delimited by a `$` before and after
* blocks: expression on a new line delimited by `$$` before and after


# Leaf block directive for more types of content

We wanted to provide a few additional blocks types (such as embed medias, nft content, videos), and have a framework to extend the spec in case we wanted to add more block types.

We decided to use the Generic directives syntax, a proposal designed to add support to a wide range of new block types with the possibility to define new block types with a cohesive syntax.

* [proposal](https://talk.commonmark.org/t/generic-directives-plugins-syntax/444)
* [syntax](https://github.com/micromark/micromark-extension-directive#syntax)

This syntax is implemented by a few parsers already, so it facilitate the integration of new blocks when required.

The syntax is the following:

```md
::name[label]{attribute1="value" attribute2="other value"}
```

See the syntax link for more details.

We added 2 directive blocks to our specification:

## ::embed-media

The embed media block can be used to embed medias from 3rd party applications by providing a valid link to some embeddeable content in a list of supported platforms. For now, we have support for:

* youtube
* twitter
* spotify

Example:

```md
::embed-media[Optionnal description]{href="https://open.spotify.com/playlist/1S1ksLs5R6Rd3aRPeOX9sW?si=a1e56e3c65fb4c56"}
```

## ::tezos-storage-pointer

To point to some data stored in tezos smart contract, we utilize our [Tezos Storage Pointer specification](../general/tezos-storage-pointers.md). It can be used to declaratively reference some content stored in a smart contract, such as a NFT. The attributes and their values in the directive are mapped to key-value pairs in the TSP spec.

Example:

```md
::tezos-storage-pointer[Nice fxhash project]{contract="KT1Sy7X6TubmZ39G8CHVrUcxjc3jiF68P8oB" path="ledger::85" value_path="" spec="FX-ISSUER-002" data_spec="FX-GEN-DATA-002"}
```

It is up to the markdown viewers to define how they want to fetch and display the data pointed by these blocks.


# Support for IPFS native urls

In order to support ipfs resources, especially in the context of displaying images stored on IPFS, we specify a native support for [ipfs native urls](https://docs.ipfs.tech/how-to/address-ipfs-on-web/#native-urls).

As such, the following markdown to declare an image:

```md
![Image alt text](ipfs://QmSDby6PefU13Q8TXc5fmhe7pd2wXCrhYGpS4LA9L5tZry)
```

Should be loaded appropriately by fx-markdown viewers by fetching the given resource through an IPFS gateway.