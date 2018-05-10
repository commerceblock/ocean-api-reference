# Ocean API reference























The Ocean client includes all of the Remote Procedure Calls (RPCs) of 
the Elements platform (described in the reference 
[here](https://github.com/ElementsProject/elementsbp-api-reference/blob/master/api.md)) 
as well as additional RPCs that control advanced and extended unique to the Ocean client. 
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
- [setcontract][]

### Policy
- [addtowhitelist][]
- [readwhitelist][]
- [removefromwhitelist][]
- [clearwhitelist][]
- [dumpwhitelist][]
- [setcontracthash][]

Configuration options

- [pkhwhitelist][]
- [disablect][]
- [bitcoinattest][]
- [bitcoinconfirmation][]
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
  
  The name of the output file for the list of tweaked addressed and base public keys
  
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
   <td markdown="contract">
   
   `contract`
   
   </td>
   <td>object</td>
   <td>Required<br />(exactly 1)</td>
   <td>A JSON object containing the plain text of the contract</td>
  </tr>

  <tr>
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
   <td markdown="contracthash">

   `contracthash`

   </td>
   <td>string</td>
   <td>Required<br />(exactly 1)</td>
   <td>The hex-encoded hash of the contract hash</td>
  </tr>

  <tr>
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

[dumpderivedkeys]: #dumpderivedkeys
[validatederivedkeys]: # validatederivedkeys
[getderivedkeys]: # getderivedkeys
[getcontract]: # getcontract
[getcontracthash]: # getcontracthash

