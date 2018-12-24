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
- [getderivedkeys][]
- [getcontract][]
- [getcontracthash][]
- [getmappinghash][]

### Utility
- [getutxoassetinfo][]
- [createrawissuance][]
- [createrawreissuance][]
- [createrawburn][]

### Policy
- [addtowhitelist][]
- [readwhitelist][]
- [readwhitelistdb][]
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
- [pkhwhitelistmongodb][]
- [wldbuser][]
- [wldbpass][]
- [wldbport][]
- [wldbhost][]
- [wldbdatabase][]
- [wldbauthsource][]
- [wldbauthmechanism][] 
- [freezelist][]
- [burnlist][]
- [issuanceblock][]
- [disablect][]
- [embedcontract][]
- [attestationhash][]
- [embedmapping][]
- [issuecontrolscript][]
- [initialfreecoinsdestination][]

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
ocean-cli validatederivedkeys
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

## getutxoassetinfo

The `getutxoassetinfo` RPC returns a summary of the total amounts of unspent (and un-burnt) 
assets in the UTXO set. Ammounts in transactions marked as frozen (i.e. with one output 
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

## addtowhitelist

The `addtowhitelist` RPC adds a valid contract tweaked address to the node
mempool whitelist. It requires both an address and corresponding base public
key, and the RPC cheacks that the address is valid and has been tweaked
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

*Result---none if valid, errors returned if invalid inoputs*

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
key, and the RPC cheacks that each address is valid and has been tweaked
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

*Result---none if valid, errors returned if invalid inoputs*

*Example*

```bash
ocean-cli readwhitelist derivedkeys.txt
```

## readwhitelistdb

The `readwhitelistdb` RPC adds the list of valid contract tweaked address to the node from a mongodb database to
mempool whitelist. It requires the database environment variables or configuration options, and a correctly formatted 
database as specified in [pkhwhitelistmongodb][].

*Result---none if valid, errors returned if invalid inoputs*

*Example*

```bash
ocean-cli readwhitelistdb
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

*Result---none if valid, errors returned if invalid inoputs*

*Example*

```bash
ocean-cli removefromwhitelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearwhitelist

The `clearwhitelist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: nome*

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

*Result---none if valid, errors returned if invalid inoputs*

*Example*

```bash
ocean-cli removefromfreezelist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearfreezelist

The `clearfreezelist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: nome*

*Example*

```bash
ocean-cli clearfreezelist
```

## addtofreezelist

The `addtoburnlist` RPC adds an address to the node
mempool burnlist. Transactions spending from UTXOs with 
output addresses on the freezelist are alowed into the 
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

*Result---none if valid, errors returned if invalid inoputs*

*Example*

```bash
ocean-cli removefromburnlist 2dZhhVmJkXCaWUzPmhmwQ3gBJm2NJSnrvyz
```

## clearburnlist

The `clearburnlist` RPC clears the mempool whitelist of all addresses.

*Parameters: none*

*Result: nome*

*Example*

```bash
ocean-cli clearburnlist
```

## Configuration options

The following configuration options are specific to Ocean. 

## pkhwhitelistmongodb

Read whitelist addresses from a mongodb database (default: false).

This option enables reading from a mongodb whitelist database. The addresses are to be stored in a collection called `whitelist`. 
Each document in the collection should have the following fields (additional fields are permitted):

{
_id: `id-0000001`, 
addresses: `[address1, address2, ..., addressN]`,
keys: `[key1, key2, ..., keyN]`
}

Here, `_id` is a unique identifier, `keys` is an array containing the public keys to be whitelisted and `addresses` is an array
containing the corresponding contract tweaked addresses. `_id` is a mandatory field; the others are optional.

If the `pkhwhitelistmongodb` option is enabled, on startup all valid addresses will be read into the local address whitelist memory 
in the same manner as the [readwhitelistdb][] RPC call. The database will also be monitored for changes; addresses will be automatically 
read in from any new documents inserted into the collection. If any documents are modified or deleted, the local whitelist 
will be resynchronised with the database. Resynchronisation is a longer process than insertion because all addresses will be reimported.

The following configuration options are required: [wldbuser][], [wldbpass][], [wldbport][], [wldbhost][], [wldbdatabase][],
[wldbauthsource][], [wldbauthmechanism][].

## wldbuser

Whitelist database username. Default: $WLDBUSER

## wldbpass

Whitelist database password. Default: $WLDBPASS

## wldbport

Whitelist database password. Default: $WLDBPORT

## wldbhost

Whitelist database password. Default: `wldbhost` (defined in /etc/hosts).

## wldbdatabase

Whitelist database name. Default: $WLDBDATABASE

## wldbauthsource

Whitelist database authsource. Default: $WLDBAUTHSOURCE

## wldbauthmechanism

Whitelist database authmechanism. Default: `SCRAM-SHA-256`

[dumpderivedkeys]: #dumpderivedkeys
[validatederivedkeys]: #validatederivedkeys
[getderivedkeys]: #getderivedkeys
[getcontract]: #getcontract
[getcontracthash]: #getcontracthash
[getmappinghash]: #getmappinghash
[createrawissuance]: #createrawissuance
[createrawreissuance]: #createrawissuance
[createrawburn]: #createrawburn
[getutxoassetinfo]: #getutxoassetinfo
[addtowhitelist]: #addtowhitelist
[readwhitelist]: #readwhitelist
[readwhitelistdb]: #readwhitelistdb
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
[pkhwhitelistmongodb]: #pkhwhitelistmongodb
[wldbuser]: #wldbuser
[wldbpass]: #wldbpass
[wldbport]: #wldbport
[wldbhost]: #wldbhost
[wldbdatabase]: #wldbdatabase
[wldbauthsource]: #wldbauthsource
[wldbauthmechanism]: #wldbauthmechanism
[freezelist]: #freezelist
[burnlist]: #burnlist
[issuanceblock]: #issuanceblock
[disablect]: #disablect
[embedcontract]: #embedcontract
[attestationhash]: #attestationhash
[embedmapping]: #embedmapping
[issuecontrolscript]: #issuecontrolscript
[initialfreecoinsdestination]: #initialfreecoinsdestination
