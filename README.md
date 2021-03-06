### ***********************************************************************
### This document is no longer maintained. Find ocean API reference [here](https://commerceblock.readthedocs.io/en/latest/ocean-api/index.html). 
### ***********************************************************************

# Ocean API reference























The Ocean client includes all of the Remote Procedure Calls (RPCs) of
the Elements platform (described in the reference
[here](https://github.com/ElementsProject/elementsbp-api-reference/blob/master/api.md))
as well as additional RPCs that control advanced and extended features unique to the Ocean client.
This document describes these new RPCs and their function as well as additional
Ocean client configuration options that enable them.

For any RPC supported in the Ocean client (including those inherited from Elements and
Bitcoin), you can get information about function and correct usage from the command line
using the `help` RPC.  For example,

```text
ocean-cli help getblockchaininfo
```

## Quick reference

The following RPCs are unique to the Ocean client

### Wallet
- [dumpderivedkeys][]
- [validatederivedkeys][]
- [dumpkycfile][]
- [readkycfile][]
- [getderivedkeys][]
- [getcontract][]
- [getcontracthash][]
- [getmappinghash][]

### Utility
- [getutxoassetinfo][]
- [createrawissuance][]
- [createrawreissuance][]
- [createrawburn][]
- [testmempoolaccept][]
- [createrawpolicytx][]
- [createrawrequesttx][]
- [createrawbidtx][]
- [getrequests][]
- [getrequestbids][]

### Policy
- [addtowhitelist][]
- [readwhitelist][]
- [querywhitelist][]
- [removefromwhitelist][]
- [clearwhitelist][]
- [dumpwhitelist][]
- [addtofreezelist][]
- [queryfreezelist][]
- [removefromfreezelist][]
- [clearfreezelist][]
- [addtoburnlist][]
- [queryburnlist][]
- [removefromburnlist][]
- [clearburnlist][]

### Configuration options

- [pkhwhitelist][]
- [freezelist][]
- [burnlist][]
- [issuanceblock][]
- [disablect][]
- [embedcontract][]
- [attestationhash][]
- [embedmapping][]
- [issuecontrolscript][]
- [initialfreecoinsdestination][]
- [freezelistcoinsdestination][]
- [burnlistcoinsdestination][]
- [permissioncoinsdestination][]

## dumpderivedkeys

The `dumpderivedkeys` RPC outputs a list of all contract tweaked
addresses in the key pool
along with the corresponding non-tweaked basis public keys to a specified file.

*Parameter #1---the filename of the output file*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>filename</td>
  <td>string</td>
  <td>Required<br />(exactly 1)</td>
  <td markdown="block">

  The name of the output file for the list of tweaked addresses and base public keys

  </td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli dumpderivedkeys dumpfile.txt
```

## validatederivedkeys

The `validatederivedkeys` RPC reads in a list of tweaked addresses with corresponding
base public keys (as produced by [dumpderivedkeys][]) from a specified file, and
then checks that the address corresponds to the corresponding public key when
tweaked with the current contract hash.

*Parameter #1---the filename of the input file*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>filename</td>
   <td>string (hex)</td>
   <td>Required<br />(exactly 1)</td>
   <td>The name of the output file for the list of tweaked addressed and base public keys</td>
  </tr>
 </tbody>
</table>

*Result---nothing if valid keys, RPC errors if invalid keys found*

*Example*

```text
ocean-cli validatederivedkeys filename.txt
```

## dumpkycfile

The `dumpkycfile` RPC outputs an encrypted list of wallet tweaked public keys. A timestamp and best block hash of when the file was constructed is included.

*Parameter #1---the filename of the output file*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>filename</td>
  <td>string</td>
  <td>Required<br />(exactly 1)</td>
  <td markdown="block">

  The name of the output file for the list of tweaked public keys

  </td>
 </tbody>
</table>

*Parameter #2---the public key with which to encrypt*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>public key</td>
  <td>string</td>
  <td>Optional<br />(0 or 1)</td>
  <td markdown="block">

  The specific public key to be used for file encryption

  </td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli dumpkycfile dumpfile.txt 1CDXUtbF3bBtritydFMKhRbbYhxDgCF5oH
```

## readkycfile

The `readkycfile` RPC reads in an encrypted list of tweaked public keys (as produced by [dumpkycfile][]) from a specified file, and outputs the unencrypted list of tweaked addresses along with their corresponding non-tweaked public keys.


*Parameter #1---the encrypted filename*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>encrypted filename</td>
  <td>string</td>
  <td>Required<br />(exactly 1)</td>
  <td markdown="block">

  The name of the file containing the encrypted tweaked public keys

  </td>
  </tr>
 </tbody>
</table>

*Parameter #2---the filename of the output file*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>filename</td>
  <td>string</td>
  <td>Required<br />(exactly 1)</td>
  <td markdown="block">

  The name of the output file for the list of tweaked public keys and coresponding addresses
  
  </td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli readkycfile dumpfile.txt dumpfileout.txt
```

## getderivedkeys

The `getderivedkeys` RPC returns a list of contract tweaked
addresses in the key pool
along with the corresponding non-tweaked basis public keys as a JSON object.

*Parameters: none*

*Result---the txid and vout of the reissuance output*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>result
   </td>
   <td>object</td>
   <td>Required<br />(exactly 1)</td>
   <td>An object containing a list of contract tweaked addresses and
basis public keys</td>
  </tr>
  <tr>
   <td markdown="block">

   →<br>`address`

   </td>

   <td>string (hex)</td>
   <td>Required<br />(key pool size)</td>
   <td>Base58check encoded address corresponding to the contract tweaked public key</td>
  </tr>
  <tr>
   <td markdown="block">

   →<br>`bpubkey`

   </td>

   <td>string (hex)</td>
   <td>Required<br />(key pool size)</td>
   <td>Hex encoding of the compressed untweaked public key</td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli getderivedkeys
```

Result:

```json
{
  "address": ["2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz","2daBDLApGapXjW4xErMsYDAHWd2QzFHHxvB"],
  "bpubkey": ["028f9c608ded55e89aef8ade69b90612510dbd333c8d63cbe1072de9049731bb58","0263a73eca5334af77037a1c8844b5220017bf6fb627c5a57c862dff20ea001d99"]
}
```

## getcontract

The `getcontract` RPC returns the plain text of the currently enforced
contract.

*Parameters: none*

*Result---the full plain text of the current contract*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>contract</td>
   <td>object</td>
   <td>Required<br />(exactly 1)</td>
   <td>A JSON object containing the plain text of the contract</td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli getcontract
```

Result:

```json
{
  "contract": "These are the current terms and conditions that govern participation in the Ocean network. 1. Be awsome to each other. 2. No smoking."
}
```

## getcontracthash

The `getcontracthash` RPC returns the hash of the contract in force at a given block height.
If the block height is not supplied, the current contract hash is returned.

*Parameter #1---the blockheight at which a contract was in force*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>block height</td>
   <td>number (int)</td>
   <td>Optional<br />(0 or 1)</td>
   <td>Block height to retrieve the contract hash from (Default: most recent block)</td>
  </tr>
 </tbody>
</table>

*Result---the contract hash*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>contracthash</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The hex-encoded hash of the contract hash</td>
  </tr>

 </tbody>
</table>

*Example*

```bash
ocean-cli getcontracthash
```

Result:

```text
f4f30db53238a7529bc51fcda04ea22bd8f8b188622a6488da12281874b71f72
```

## getmappinghash

The `getmappinghash` RPC returns the hash of the mapping object in force at a given block height.
If the block height is not supplied, the current mapping hash is returned.

*Parameter #1---the blockheight at which a mapping was in force*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>block height</td>
   <td>number (int)</td>
   <td>Optional<br />(0 or 1)</td>
   <td>Block height to retrieve the mapping hash from (Default: most recent block)</td>
  </tr>
 </tbody>
</table>

*Result---the mapping hash*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>mappinghash</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The hex-encoded hash of the mapping hash</td>
  </tr>

 </tbody>
</table>

*Example*

```bash
ocean-cli getmappinghash
```

Result:

```text
f4f30db53238a7529bc51fcda04ea22bd8f8b188622a6488da12281874b71f72
```

## createrawissuance

The `createrawissuance` RPC creates an unblinded raw unsigned issuance transaction with specified
outputs and spending from a specified input containing an amount of policy asset.

*Parameter #1---the Base58check address for the issued asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>assetaddress</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check issued asset address</td>
  </tr>
 </tbody>
</table>

*Parameter #2---the amount of issued asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>assetamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of asset to issue</td>
  </tr>
 </tbody>
</table>

*Parameter #3---the Base58check address for the reissuance token*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>tokenaddress</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check reissuance token address</td>
  </tr>
 </tbody>
</table>

*Parameter #4---the amount of reissuance token*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>tokenamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of reissuance token</td>
  </tr>
 </tbody>
</table>

*Parameter #5---the Base58check address for the policyAsset change*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>changeaddress</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check change address</td>
  </tr>
 </tbody>
</table>

*Parameter #6---the amount of policyAsset change*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>changeamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of policyAsset to return per output</td>
  </tr>
 </tbody>
</table>

*Parameter #7---the number of policyAsset outputs*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>numchange</td>
   <td>integer</td>
   <td>Required<br />(exactly 1)</td>
   <td>Numebr of policyAsset outputs</td>
  </tr>
 </tbody>
</table>

*Parameter #8---input TXID*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>inputtxid</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Hex of the input TXID containing the policyAsset</td>
  </tr>
 </tbody>
</table>

*Parameter #9---input transaction vout*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vout</td>
   <td>integer</td>
   <td>Required<br />(exactly 1)</td>
   <td>Input transaction vout</td>
  </tr>
 </tbody>
</table>

*Result---the unsigned raw transaction in hex*

*Example*

```bash
ocean-cli createrawissuance 2deJ6F3w6HUtXM8JjY5YPc8wtaXerqFX7HA 123.0 2ddSmTujABoCzDyU9hPghcp4ojAGmJRtnWk 1.23 XKSxznoA799169xt3zCm7a4qkdT1KZANv3 332.9995 1 0.0005 40ac4e02a64ea14190e96d6e1c5c877b12522db3fb5adffd58b1aed0cc11150a 0
```

Result:

```text
0200000000010a1511ccd0aeb158fddf5afbb32d52127b875c1c6e6de99041a14ea6024eac400000008000ffffffff000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000002dd231b0001000000000754d4c0040174820afc79a50ed4b9de7bfbc286adead7d46300787e3d986dd8bff570902d620100000002dd231b00001976a9143565dfe051b252c6c5d51275fa91850ef0bb26b288ac017c2096b20b81bd5734cb631335fa1a5bfed9c5a10e2ba4ce86df1b28fb2d50e401000000000754d4c0001976a9142c11d12a57cdd294b3ffa89cf2503a612252fbf288ac01230f4f5d4b7c6fa845806ee4f67713459e1b69e8e60fcee2e4940c7a0d5de1b20100000007c0d4e9b00017a91458d6453537165062f887d004b979b054c5fa98c28701230f4f5d4b7c6fa845806ee4f67713459e1b69e8e60fcee2e4940c7a0d5de1b201000000000000c350000000000000
```

## createrawreissuance

The `createrawreissuance` RPC creates a raw unsigned re-issuance (asset inflation) transaction with specified
outputs and spending from a specified input containing a valid re-issuance token.

*Parameter #1---the Base58check address for the re-issued asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>assetaddress</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check re-issued asset address</td>
  </tr>
 </tbody>
</table>

*Parameter #2---the amount of re-issued asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>assetamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of asset to re-issue</td>
  </tr>
 </tbody>
</table>

*Parameter #3---the Base58check address for the return reissuance token*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>tokenaddress</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check reissuance token address</td>
  </tr>
 </tbody>
</table>

*Parameter #4---the amount of reissuance token to return*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>tokenamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of reissuance token</td>
  </tr>
 </tbody>
</table>

*Parameter #5---input TXID*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>inputtxid</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Hex of the input TXID containing the re-issuance token</td>
  </tr>
 </tbody>
</table>

*Parameter #6---input transaction vout*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vout</td>
   <td>integer</td>
   <td>Required<br />(exactly 1)</td>
   <td>Input transaction vout</td>
  </tr>
 </tbody>
</table>

*Parameter #7---the asset entropy*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>entropy</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The hex encoded asset entropy</td>
  </tr>
 </tbody>
</table>

*Result---the unsigned raw transaction in hex*

*Example*

```bash
ocean-cli createrawissuance 2deJ6F3w6HUtXM8JjY5YPc8wtaXerqFX7HA 0.5 2ddSmTujABoCzDyU9hPghcp4ojAGmJRtnWk 1.00 40ac4e02a64ea14190e96d6e1c5c877b12522db3fb5adffd58b1aed0cc11150a 0 b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23
```

Result:

```text
0200000000010a1511ccd0aeb158fddf5afbb32d52127b875c1c6e6de99041a14ea6024eac400000008000ffffffff000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000002dd231b0001000000000754d4c0040174820afc79a50ed4b9de7bfbc286adead7d46300787e3d986dd8bff570902d620100000002dd231b00001976a9143565dfe051b252c6c5d51275fa91850ef0bb26b288ac017c2096b20b81bd5734cb631335fa1a5bfed9c5a10e2ba4ce86df1b28fb2d50e401000000000754d4c0001976a9142c11d12a57cdd294b3ffa89cf2503a612252fbf288ac01230f4f5d4b7c6fa845806ee4f67713459e1b69e8e60fcee2e4940c7a0d5de1b20100000007c0d4e9b00017a91458d6453537165062f887d004b979b054c5fa98c28701230f4f5d4b7c6fa845806ee4f67713459e1b69e8e60fcee2e4940c7a0d5de1b201000000000000c350000000000000
```

## createrawburn

The `createrawburn` RPC creates a raw unsigned burn (OP_RETURN) transaction with a single input and single output.

*Parameter #1---input TXID*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>txid</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Hex of the input TXID containing the burn asset</td>
  </tr>
 </tbody>
</table>

*Parameter #2---input transaction vout*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>vout</td>
   <td>integer</td>
   <td>Required<br />(exactly 1)</td>
   <td>Input transaction vout</td>
  </tr>
 </tbody>
</table>

*Parameter #3---the asset type*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>asset</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The hex encoded asset id to be burnt</td>
  </tr>
 </tbody>
</table>

*Parameter #4---the amount to burn (must equal the input amount)*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>burnamount</td>
   <td>amount</td>
   <td>Required<br />(exactly 1)</td>
   <td>Amount of burnt asset</td>
  </tr>
 </tbody>
</table>

*Result---the unsigned raw transaction in hex*

*Example*

```bash
ocean-cli createrawissuance 40ac4e02a64ea14190e96d6e1c5c877b12522db3fb5adffd58b1aed0cc11150a 0 b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23 0.342100
```

Result:

```text
0200000000010a1511ccd0aeb158fddf5afbb32d52127b875c1c6e6de99041a14ea6024eac400000008000ffffffff000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000002dd231b0001000000000754dee2e4940c7a0d5de1b201000000000000c350000000000000
```

## createrawbidtx

The `createrawbidtx` RPC creates a raw bid transaction with a single input and multiple outputs.

*Parameter #1---Input object with details on transaction to be spent*

*Parameter #2---Output object with bid and request details*

*Result---the unsigned raw transaction in hex*

*Example*

```bash
ocean-cli createrawbidtx '''[
  {
    "txid": "43bd75af773cce38fd190f6c0943d311ce2dd8a26c7e7a9e600c58f8b21e53d4",
    "vout": 1,
    "asset": "aaad75af773cce38fd190f6c0943d311ce2dd8a26c7e7a9e600c58f8b21e53d4"
  }
]''' '''[
  {
    "pubkey": "03d5be1ca0b06b54f6a29a8e245fdf58698164538191c5b376d3b27e6d3229b81a",
    "value": 1000.0,
    "change": 500.0,
    "changeAddress": "2drB6JQJTCe9bWc8NQ7cBjUsQejkJWs3ptg",
    "fee": 5,
    "endBlockHeight": 250,
    "requestTxid": "d79b437a45a3d7343fb58c05163e04604a9cdb62a8e454dd1292d6645578043a",
    "feePubkey": "03f6be1ca0b06b54f6a29a8e245fdf58698164538191c5b376d3b27e6d3229b81a",
  }
]'''
```

Result:

```text
02000000000151227925212487ef62c10e46f14aec78dce956b02eb41f7e2cce8b6d56292db40100000000feffffff0101d08413554d89a69f0d93a6f7e33242d472a6503b11b1b7c10d3134afb2a36d0101000000174876e800006d0169b17551210246e99744bdee2ce153eb6185019e73ff6bb4d050d50d1a1d7d773f45474d4c362102051d7e7caa636a8fb1eaca4fb163248aff57d5e4e04e84734101b138e1a07d862103640000000a0000000a000000010000000000000000000000000000000000000053ae66000000
```

## createrawrequesttx

The `createrawrequesttx` RPC creates a raw request transaction with a single input and single output.

*Parameter #1---Input object with details on transaction to be spent*

*Parameter #2---Output object with request details*

*Result---the unsigned raw transaction in hex*

*Example*

```bash
ocean-cli createrawrequesttx '''[
  {
    "txid": "43bd75af773cce38fd190f6c0943d311ce2dd8a26c7e7a9e600c58f8b21e53d4",
    "vout": 1,
  }
]''' '''[
  {
    "pubkey": "03d5be1ca0b06b54f6a29a8e245fdf58698164538191c5b376d3b27e6d3229b81a",
    "decayConst": 5,
    "endBlockHeight": 250,
    "fee": 5,
    "genesisBlockHash": "99bd75af773cce38fd190f6c0943d311ce2dd8a26c7e7a9e600c58f8b21e53d4",
    "startBlockHeight": 100,
    "tickets": 150,
    "value": 1000.0,
    "startPrice": 5.0
  }
]'''
```

Result:

```text
02000000000151227925212487ef62c10e46f14aec78dce956b02eb41f7e2cce8b6d56292db40100000000feffffff0101d08413554d89a69f0d93a6f7e33242d472a6503b11b1b7c10d3134afb2a36d0101000000174876e800006d0169b17551210246e99744bdee2ce153eb6185019e73ff6bb4d050d50d1a1d7d773f45474d4c362102051d7e7caa636a8fb1eaca4fb163248aff57d5e4e04e84734101b138e1a07d862103640000000a0000000a000000010000000000000000000000000000000000000053ae66000000
```

## testmempoolaccept

The `testmempoolaccept` RPC determines the validity of a raw transaction without broadcasting it. It performs the exact same validity checks as performed on mempool acceptance, including locally configured policy rules, but without adding the transaction to the mempool.

*Parameter #1---signed raw transaction*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>rawtx</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Hex encoded serialised transaction</td>
  </tr>
 </tbody>
</table>

*Parameter #2---accept large fee*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>nLargeFee</td>
   <td>boolean</td>
   <td>Optional<br />(0 or 1)</td>
   <td>Allow large fee</td>
  </tr>
 </tbody>
</table>

*Result---a JSON object containing the transaction ID, whether the transaction is accepted or rejected, and if rejected the reason*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>txid</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The raw transaction ID</td>
  </tr>
 </tbody>
 <tbody>
  <tr>
   <td>allowed</td>
   <td>boolean</td>
   <td>Required<br />(exactly 1)</td>
   <td>The determination on whether the transaction is valid/allowed</td>
  </tr>
 </tbody>
 <tbody>
  <tr>
   <td>reject-reason</td>
   <td>string</td>
   <td>Optional<br />(0 or 1)</td>
   <td>The reason if the transaction would be rejected</td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli testmempoolaccept 0200000000010a1511ccd0aeb158fddf5afbb32d52127b875c1c6e6de99041a14ea6024eac400000008000ffffffff000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000002dd231b0001000000000754dee2e4940c7a0d5de1b201000000000000c350000000000000
```

Result:

```json
{
 "txid": 40ac4e02a64ea14190e96d6e1c5c877b12522db3fb5adffd58b1aed0cc11150a
 "accept": 1
}
```

## createrawpolicytx

The `createrawpolicytx` RPC creates a raw unsigned policy transaction that encodes an address to be added to a policy list. To be accepted, the asset type must match the policy asset type as defined in the genesis block (via `-freezelistcoinsdestination` and `-burnlistcoinsdestination`. The policy asset input(s) are specified in an array, and the outputs are specified in an array of objects that contain a policy public key, the address to be added to to the policy list and the value. Spending these outputs removes the addresses from the policy lists.

*Parameter #1---Inputs*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td markdown="span">
   Inputs
   </td>
   <td markdown="span">
   array
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   An array of objects, each one to be used as an input to the transaction

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → Input
   </td>
   <td markdown="span">
   object
   </td>
   <td markdown="span">
   Required<br>(1 or more)
   </td>
   <td markdown="span">

   An object describing a particular input

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`txid`
   </td>
   <td markdown="span">
   string (hex)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   The TXID of the outpoint to be spent encoded as hex in RPC byte order

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`vout`
   </td>
   <td markdown="span">
   number (int)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   The output index number (vout) of the outpoint to be spent; the first output in a transaction is index `0`

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`Sequence`
   </td>
   <td markdown="span">
   number (int)
   </td>
   <td markdown="span">
   Optional<br>(0 or 1)
   </td>
   <td markdown="span">

   The sequence number to use for the input

   </td>
  </tr>
 </tbody>
</table>

*Parameter #1---Addresses to add to policy lists and control keys*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td markdown="span">
   Policy outputs
   </td>
   <td markdown="span">
   array
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   An array of objects, each one is used to add an address to a policy list

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → Input
   </td>
   <td markdown="span">
   object
   </td>
   <td markdown="span">
   Required<br>(1 or more)
   </td>
   <td markdown="span">

   An object encoding an output with a policy list address

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`pubkey`
   </td>
   <td markdown="span">
   string (hex)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   The public key of the policy authority wallet used to spend the output

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`value`
   </td>
   <td markdown="span">
   number (coins)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   The amount of policy asset to be sent to the output

   </td>
  </tr>
  <tr>
   <td markdown="span">
   → →<br>`address`
   </td>
   <td markdown="span">
   string (base58check)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   The address to be added to the policy list

   </td>
  </tr>
 </tbody>
</table>

*Parameter #3---locktime*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td markdown="span">
   Locktime
   </td>
   <td markdown="span">
   numeric (int)
   </td>
   <td markdown="span">
   Required<br>(exactly 1)
   </td>
   <td markdown="span">

   Indicates the earliest time (in block height or Unix epoch time) a transaction can be added to the block chain

   </td>
  </tr>
 </tbody>
</table>

*Parameter #4---policy asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td markdown="span">

   Policy asset

   </td>

   <td markdown="span">

   string (hex)

   </td>

   <td markdown="span">

   Required<br>(exactly 1)

   </td>

   <td markdown="span">

   The 32 byte asset ID of the policy asset of the list being updated

   </td>

  </tr>
 </tbody>
</table>

*Result---the unsigned raw transaction in hex*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td markdown="span">

   `result`

   </td>

   <td markdown="span">

   string

   </td>

   <td markdown="span">

   Required<br>(Exactly 1)

   </td>

   <td markdown="span">

   The resulting unsigned raw transaction in serialized transaction format, encoded as hex.  If the transaction couldn't be generated, this will be set to JSON `null` and the JSON-RPC error field may contain an error message

   </td>

  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli createrawpolicytx '''[
  {
    "txid": "43bd75af773cce38fd190f6c0943d311ce2dd8a26c7e7a9e600c58f8b21e53d4",
    "vout": 1
  }
]''' '''[
  {
    "pubkey": "03d5be1ca0b06b54f6a29a8e245fdf58698164538191c5b376d3b27e6d3229b81a",
    "value": 10.0
    "address": "1HPkc4to3GzVcEV8Le6sS4V5AXWQceH5kZ"
  }
]'''
```

Result:

```text
0200000000018eb8f2e93d81b9f904f8613b6520460c00f9313683b5574fc8b67c3e33e615550000000000ffffffff010131e34d9c314f12348e8b2cb401d07450ef5e831d11bba35bd9b124f52e78387301000000746a5288000047512103e6ec6419791bcdc9ec5b2cf7d25cfffa244f2cddfd9997dffbd002c4d88baabe21020000000000000000000000005923c34a75800d2e9a6894d846649b65995dc84252ae00000000
```

## getutxoassetinfo

The `getutxoassetinfo` RPC returns a summary of the total amounts of unspent (and un-burnt)
assets in the UTXO set. Amounts in transactions marked as frozen (i.e. with one output
having a zero address) are listed in a separate field.

*Parameters: none*

*Result---an array of JSON objects containing the unspent amounts for each issued asset*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>contract</td>
   <td>object</td>
   <td>Required<br />(exactly 1)</td>
   <td>An array of JSON objects of unspent amounts for each issued asset</td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli getutxoassetinfo
```

Result:

```json
[
  {
    "asset": "7f7c00ca515e46165ea6a13dd49d22759beeb26a952128f1a5af824d208a051e",
    "spendabletxouts": 1,
    "amountspendable": 3.00000000,
    "frozentxouts": 0,
    "amountfrozen": 0.00000000
  },
  {
    "asset": "b2e15d0d7a0c94e4e2ce0fe6e8691b9e451377f6e46e8045a86f7c4b5d4f0f23",
    "spendabletxouts": 107,
    "amountspendable": 500000.00000000,
    "frozentxouts": 0,
    "amountfrozen": 0.00000000
  }
]

```
## getrequestbids

The `getrequestbids` RPC returns the request and successful bids

*Parameter #1---the request transaction hash*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>transaction hash</td>
   <td>hash (string)</td>
   <td>Mandatory</td>
   <td>The transaction hash to filter requests by</td>
  </tr>
 </tbody>
</table>

*Result---a JSON object containing details for the active request and bids*

*Example*

```bash
ocean-cli getrequestbids 123450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05
```

Result:

```json
[
  {
    "genesisBlock": "666450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05",
    "startBlockHeight": 105,
    "numTickets": 20,
    "decayConst": 2,
    "startPrice": 5.0,
    "auctionPrice": 4.8,
    "feePercentage": 5,
    "endBlockHeight": 350,
    "confirmedBlockHeight": 35,
    "txid": "123450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05",
    "bids": [
        {
            "txid": "8e5ebb572e5b410369af7f818aeee2ee313aca04c3325c407fe315c23fb73cc5",
            "feePubKey": "0300adf7a8f55f92f8be6a5ed7619d1821c5bc9901f5592badea04677043b83656"
        },
    ]
  },
]
```

## getrequests

The `getrequests` RPC returns all the active client requests in the blockchain

*Parameter #1---the client genesis block hash*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>genesis block hash</td>
   <td>hash (string)</td>
   <td>Optional</td>
   <td>The genesis block hash to filter requests by</td>
  </tr>
 </tbody>
</table>

*Result---an array of JSON objects containing details for each active request*

*Example*

```bash
ocean-cli getrequests
ocean-cli getrequests 123450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05
```

Result:

```json
[
  {
    "genesisBlock": "123450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05",
    "startBlockHeight": 105,
    "numTickets": 20,
    "decayConst": 2,
    "startPrice": 5.0,
    "auctionPrice": 4.8,
    "feePercentage": 5,
    "endBlockHeight": 350,
    "confirmedBlockHeight": 35,
    "txid": "666450e138b1014173844ee0e4d557ff8a2463b14fcaeab18f6a63aa7c7e1d05"
  },
]
```

## addtowhitelist

The `addtowhitelist` RPC adds a valid contract tweaked address to the node
mempool whitelist. It requires both an address and corresponding base public
key, and the RPC checks that the address is valid and has been tweaked
from the supplied base public key with the current
contract hash as present in the most recent block header.

*Parameter #1---the Base58check contract tweaked address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded contract tweaked address</td>
  </tr>
 </tbody>
</table>

*Parameter #2---the base (un-tweaked) compressed public key*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>bpubkey</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Hex encoded base public key</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli addtowhitelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz 028f9c608ded55e89aef8ade69b90612510dbd333c8d63cbe1072de9049731bb58
```

## querywhitelist

The `querywhitelist` RPC queries if a specified address is present in the node mempool whitelist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---TRUE of FALSE*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>output</td>
   <td>boolian</td>
   <td>Required<br />(exactly 1)</td>
   <td>1 is the address is present, 0 otherwise</td>
  </tr>

 </tbody>
</table>

*Example*

```bash
ocean-cli querywhitelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

Result:

```text
1
```

## readwhitelist

The `readwhitelist` RPC adds a list of valid contract tweaked address to the node
mempool whitelist. It requires a file that contains a list of both an address and corresponding base public
key, and the RPC checks that each address is valid and has been tweaked
from the supplied base public key with the current
contract hash as present in the most recent block header. The file format is as decribed in [dumpderivedkeys][].

*Parameter #1---the filename to read in the list*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>filename</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Name of file containing the list of contract tweaked address and base public keys</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli readwhitelist derivedkeys.txt
```

## removefromwhitelist

The `removefromwhitelist` RPC removes a specified address from the node mempool whitelist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli removefromwhitelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearwhitelist

The `clearwhitelist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: none*

*Example*

```bash
ocean-cli clearwhitelist
```

## dumpwhitelist

The `dumpwhitelist` RPC outputs a list of all
addresses in the node mempool whitelist to a specified file.

*Parameter #1---the filename of the output file*

<table>
<thead>
 <tr>
  <th>Name</th>
  <th>Type</th>
  <th>Presence</th>
  <th>Description</th>
 </tr>
</thead>
<tbody>
 <tr>
  <td>filename</td>
  <td>string</td>
  <td>Required<br />(exactly 1)</td>
  <td markdown="block">

  The name of the output file for the list of whitelist addresses

  </td>
  </tr>
 </tbody>
</table>

*Example*

```bash
ocean-cli dumpwhitelist dumpfile.txt
```

## addtofreezelist

The `addtofreezelist` RPC adds an address to the node
mempool freezelist. Transactions spending from UTXOs with
output addresses on the freezelist are blocked from entering the
mempool if the '-freezelist' configuration option is enabled.

*Parameter #1---the Base58check address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check address</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli addtofreezelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```
## queryfreezelist

The `queryfreezelist` RPC queries if a specified address is present in the node mempool freezelist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---TRUE of FALSE*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>output</td>
   <td>boolian</td>
   <td>Required<br />(exactly 1)</td>
   <td>1 is the address is present, 0 otherwise</td>
  </tr>

 </tbody>
</table>

*Example*

```bash
ocean-cli queryfreezelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

Result:

```text
1
```

## removefromfreezelist

The `removefromfreezelist` RPC removes a specified address from the node mempool freezelist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli removefromfreezelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearfreezelist

The `clearfreezelist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: none*

*Example*

```bash
ocean-cli clearfreezelist
```

## addtoburnlist

The `addtoburnlist` RPC adds an address to the node
mempool burnlist. Transactions spending from UTXOs with
output addresses on the freezelist are allowed into the
mempool if these addresses are also on the burnlist and have
only TX_FEE and TX_NULL_DATA outputs (with both the `-freezelist`
and `-burnlist` configurations options enabled).

*Parameter #1---the Base58check address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check address</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli addtoburnlist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```
## queryburnlist

The `queryburnlist` RPC queries if a specified address is present in the node mempool burnlist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---TRUE of FALSE*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>output</td>
   <td>boolian</td>
   <td>Required<br />(exactly 1)</td>
   <td>1 is the address is present, 0 otherwise</td>
  </tr>

 </tbody>
</table>

*Example*

```bash
ocean-cli queryburnlist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

Result:

```text
1
```

## removefromburnlist

The `removefromburnlist` RPC removes a specified address from the node mempool burnlist.

*Parameter #1---the Base58check encoded address*

<table>
 <thead>
  <tr>
   <th>Name</th>
   <th>Type</th>
   <th>Presence</th>
   <th>Description</th>
  </tr>
 </thead>
 <tbody>
  <tr>
   <td>address</td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>Base58check encoded address</td>
  </tr>
 </tbody>
</table>

*Result---none if valid, errors returned if invalid inputs*

*Example*

```bash
ocean-cli removefromburnlist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearburnlist

The `clearburnlist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: none*

*Example*

```bash
ocean-cli clearburnlist
```

[dumpderivedkeys]: #dumpderivedkeys
[validatederivedkeys]: #validatederivedkeys
[dumpkycfile]: #dumpkycfile
[readkycfile]: #readkycfile
[getderivedkeys]: #getderivedkeys
[getcontract]: #getcontract
[getcontracthash]: #getcontracthash
[getmappinghash]: #getmappinghash
[createrawissuance]: #createrawissuance
[createrawreissuance]: #createrawissuance
[createrawburn]: #createrawburn
[createrawrequesttx]: #createrawrequesttx
[createrawbidtx]: #createrawbidtx
[testmempoolaccept]: #testmempoolaccept
[createrawpolicytx]: #createrawpolicytx
[getutxoassetinfo]: #getutxoassetinfo
[getrequests]: #getrequests
[getrequestbids]: #getrequestbids
[addtowhitelist]: #addtowhitelist
[readwhitelist]: #readwhitelist
[querywhitelist]: #querywhitelist
[removefromwhitelist]: #removefromwhitelist
[clearwhitelist]: #clearwhitelist
[dumpwhitelist]: #dumpwhitelist
[addtofreezelist]: #addtofreezelist
[queryfreezelist]: #queryfreezelist
[removefromfreezelist]: #removefromfreezelist
[clearfreezelist]: #clearfreezelist
[addtoburnlist]: #addtoburnlist
[queryburnlist]: #queryburnlist
[removefromburnlist]: #removefromburnlist
[clearburnlist]: #clearburnlist
[pkhwhitelist]: #pkhwhitelist
[freezelist]: #freezelist
[burnlist]: #burnlist
[issuanceblock]: #issuanceblock
[disablect]: #disablect
[embedcontract]: #embedcontract
[attestationhash]: #attestationhash
[embedmapping]: #embedmapping
[issuecontrolscript]: #issuecontrolscript
[initialfreecoinsdestination]: #initialfreecoinsdestination
[freezelistcoinsdestination]: #initialfreecoinsdestination
[burnlistcoinsdestination]: #initialfreecoinsdestination
