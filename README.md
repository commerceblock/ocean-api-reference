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

### Policy
- [addtowhitelist][]
- [readwhitelist][]
- [querywhitelist][]
- [removefromwhitelist][]
- [clearwhitelist][]
- [dumpwhitelist][]

Configuration options

- [pkhwhitelist][]
- [disablect][]
- [embedcontract][]

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
   <td>Block height to retreive the contract hash from (Default: most recent block)</td>
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


[dumpderivedkeys]: #dumpderivedkeys
[validatederivedkeys]: #validatederivedkeys
[getderivedkeys]: #getderivedkeys
[getcontract]: #getcontract
[getcontracthash]: #getcontracthash
[addtowhitelist]: #addtowhitelist
[readwhitelist]: #readwhitelist
[querywhitelist]: #querywhitelist
[removefromwhitelist]: #removefromwhitelist
[clearwhitelist]: #clearwhitelist
[dumpwhitelist]: #dumpwhitelist
[pkhwhitelist]: #pkhwhitelist
[disablect]: #disablect
[embedcontract]: #embedcontract

