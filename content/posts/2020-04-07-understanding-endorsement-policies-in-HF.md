---
date: "2020-04-07T00:00:00Z"
subtitle: Notes taken while learning about Endorsement Policies
tags:
- hyperledger
- blockchain
- distributed systems
title: Understanding Endorsement Policies in Hyperledger Fabric
---

### Introduction
Endorsement Policy defines which peers should execute a transaction and endorse its result(signing the execution result with its certificates) 
so as to consider the transaction to be valid. In other words, only after a set of peers endorse a transaction's result then only
the transaction will be committed in the ledger. *This is my attempt to learn aspects of endorsement policy with more clarity*

### Where is it defined
Well, endorsement policies can be defined at three different levels
- Chaincode Level
- Collection Level
- Key Level

### Syntax
The Endorsement policies is expressed in terms of `MSP_ID.ROLE`(also known as *principal*). The sytax for
a endorsement policy is `EXPR(E[, E...]`
Some Examples are:-
- `AND('Org1.member', 'Org2.member', 'Org3.member')` or `OutOf(3, 'Org1.member', 'Org2.member', 'Org3.member')`
Means signature is required from all the three principals
- `OR('Org1.member', 'Org2.member')` or `OutOf(1, 'Org1.member', 'Org2.member')`
Means signature is requried from either of the principals

### Endorsement Policy at Chaincode Level
Endorsement policies defined at chaincode level is agreed at the time of approval
of the chaincode definition. By default, this needs to be approved by majority 
of he channel members. Now the question is how do you define this ? There are 2 ways
- Use Fabric SDK APIs
- Use CLI using `—-signature-policy` while committing the chaincode

If none of the above is done, then by default endorsement policy would be 
that majority of channel members need to endorse.

### What are Private data Collection
Suppose within a channel, you want a transaction to occur, which must be known 
to only a few members of the channel. One way of achieving it would be to create a
furthur channel, but that would cause excess burden of maintaining chaincode, MSPs etc.
Private data Collection is the solution for exchanging private data within a channel.
Here only the members defined in the *collection* would store data in a private DB also called 
as *side DB*. Also every peer stores the hash of the data in its ledger. Note that the data
passes from one member to other without the ordering service. Even the ordering services get 
the hash of the data. Hence private data collections can be used when you want to keep the
data confidential from the ordering services. Also, forgot to mention another feature, it also allows
a part of the transactional data to be confidential from few members of the channel.

### Endorsement Policy at Collection Level
Endorsement Policy at Collection Level can be defined at the place where collections are defined,which is generally in a JSON file. And example would look something like this(example taken from HF docs):
```
{
    "name": "collectionMarblePrivateDetails",
    "policy": "OR('Org1MSP.member')",
    "requiredPeerCount": 0,
    "maxPeerCount": 3,
    "blockToLive":3,
    "memberOnlyRead": true,
    "memberOnlyWrite":true,
    "endorsementPolicy": {
      "signaturePolicy": "OR('Org1MSP.member')"
    }
 }
```
The last key is the endorsement policy defined for the private data collection named "collectionMarblePrivateDetails". Do not confuse the first key 'policy' with endorsement policy. The value of the key 'policy' defines defines which organizations peers are allowed to persist the collection data. This  collection level endorsement policy can be utlized to override the chaincode level endorsement policy.

### Endorsement Policy at Key Level
Endorsement Policies at Key Level can be defined from within the chaincode. For example, the Go Shim API provides basic functions like `SetStateValidationParameter(key string, ep []byte) error`, `GetStateValidationParameter(key string) ([]byte, error)`, `SetPrivateDataValidationParameter(collection, key string, ep []byte) error`, `GetPrivateDataValidationParameter(collection, key string) ([]byte, error)`.
There are also extension provided by the API like
```
type KeyEndorsementPolicy interface {
    // Policy returns the endorsement policy as bytes
    Policy() ([]byte, error)

    // AddOrgs adds the specified orgs to the list of orgs that are required
    // to endorse
    AddOrgs(roleType RoleType, organizations ...string) error

    // DelOrgs delete the specified channel orgs from the existing key-level endorsement
    // policy for this KVS key. If any org is not present, an error will be returned.
    DelOrgs(organizations ...string) error

    // ListOrgs returns an array of channel orgs that are required to endorse changes
    ListOrgs() ([]string)
}
```
Adding orgs to a key level Endorsement Policy is as simple as adding org's MSP IDs to the `AddOrgs()`, call `Policy()` to construct the byte array and pass it in the `SetStateValidationParameter()`.


*This was about defining Endorsement Policies at different level, for more refer to [docs](https://hyperledger-fabric.readthedocs.io/en/release-2.0/endorsement-policies.html)*
