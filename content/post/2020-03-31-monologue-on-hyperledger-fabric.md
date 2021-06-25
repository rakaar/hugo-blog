---
date: "2020-03-31T00:00:00Z"
subtitle: Just organising my notes that I made while learning Fabric
tags:
- hyperledger
- blockchain
- distributed systems
title: Monologue on Hyperledger Fabric
---

### Public vs Private blockchains

Blockchains when started out were open. Open in the sense, any individual can join the network, read , write and even verify the transactions. Also all the transactions that had happened in the blockchain would be visible to everyone in the network.

This type of model has been a great success for CryptoCurrencies and we have seen popular examples like Bitcoin and other alt coins. Ethereum is one such project which provides a platform to build Decentralised applications.

However, this is not always the case we want. Sometimes we need to restrict the entry of nodes in the network, meaning only authorised individuals should be able to join a network. And also sometimes transaction in a network need not be known to all the individuals. Only the concerned parties need to update the transaction in their ledger. Along with that we may require a condition where we can't give the transaction verification rights to all the individuals. Problems like these are solved by Private Blockchains. One such popular project is Hyperledger, which under its umbrella has lead to many Private DLT platforms like Hyperledger- fabric, Indy, Sawtooth, Iroha and many more.

Here I would like speak about Hyperledger Fabric

### Key Words

- **Ledger**: Ledger contains the data of all transactions. It consists  of 2 things: 
	-  *Blockchain* - the continuous hashed chains of blocks, where each block contains list of transactions. 
	- *World State* - the state of the most recent transaction. In simpler words, some state which can be computed from the  blockchain, for example in a cryptocurrency state can be a balance , which gets updated on a transaction 
- **Peer**: Individual in a network
- **Channels**: An independent collection of nodes in a network who share the same state of ledger
- **Membership Service**: Service that provides  notion of identity to the peers , which in case is a digital certificate which will be used to sign the transaction.
- **Certificate Authority**: The certificates discussed above are issued by the Certificate Authority. 
- **SDK**: Software which is used to interact your application with the network
- **Ordering Service**: Manage Concurrency problem. As mentioned in the name, its goal is to provide a ordered set of transactions it receives. Hence ensuring that all the peers maintain the same state of ledger in a channel.
- **Chaincode**: The business logic written in code.Chaincodes can be written in these 3 languages currently nodejs, Go, Java. 
- There are 2 kinds of peers - 
	- *Endorsing Peer*: Peers that carry a copy of the chaincode and execute it and it signs the output of the chaincode execution. Furthur passing it to the ordering Service.
	- *Committers*:  Peers that receive transactions and commit them to the Ledger. They do not execute the chaincode. 

### Transaction Flow

1. **Propose Transaction**:  A peer using its client application proposes a transaction hence proposing to invoke a part of chaincode with certain inputs and this is sent to the endorsing Peers. Does it send to all the peers. No ! It only sends to the peers which are part of Endorsement Policy which it defines along with chaincode.
2. **Executing Propsed Transaction**: The endorsers execute the proposal received and sign it
3. **Return the signed response**: Returning back the signed transaction to the client application by the endorsing peers after executing and signing.
4. **Sending to the Orderer**: Responses are sent from the client to the ordering service. Now this ordering service receives many such responses from these client applications. Now the ordering service will determine the order of transactions that need to be saved in the ledger of nodes
5. **Delivering the transactions**: Now the ordered set of transactions are sent to all the peers. This ordered set of transactions  is referred as 'block' in blockchain
6. **Validate Transactions**: Committing and endorsement peers validate the set of transactions received. A transaction may fail due to various reasons like endorsement policy not being satisfied or if the transaction is trying to mutate an invalid state.
7. **Committing and Emitting events**:- The set of valid transactions are added to the peer's ledger and the peers emit events. These events are helpful so as to notify the application that a particular operation has been performed. Client application may subscribe to particular kinds of events too so as to notify the user as per his/her requirement.

Note that here I have ignored some complexities like.Sometimes Peers are not an individual entity in a network. Instead it is organisation which can be sometimes considered as an individual entity. Each organisation may multiple peers. 
Regarding the ordering service, the ordering service can be a single node,which is called SOLO.  Recently the ordering service can also be implemented with Kafka, an open source pub-sub messaging service by Apache, which support Crash Fault Tolerance by which it means the distributed nodes in Kafka can cooperate can work fine even in case of failure of nodes. In fact,in Kafka even a single surviving node can operate perfectly.
 
### References: -
- https://www.youtube.com/watch?v=nBXr7dLXAbE&list=PLbRMhDVUMngfxxyVLh2t2gKDUfsOdGn56&index=22
- https://medium.com/coinmonks/demystifying-hyperledger-fabric-1-3-fabric-architecture-a2fdb587f6cb
