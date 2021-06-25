---
date: "2020-04-04T00:00:00Z"
subtitle: Short notes on Hyperledger Fabric Build Your First Network example setup
tags:
- hyperledger
- blockchain
- distributed systems
title: Understanding Hyperledger fabric byfn setup briefly
---

### Introduction
When I first go started with Hyperledger Fabric, it was a very daunting experience. All those yaml files, docker containers, commands,
shell scripts confused a lot. Here in this article, I try to put those things very shortly on how a network is setup, mainly will consider
the example of [BYFN Hyperledger Fabric](https://github.com/hyperledger/fabric-samples/tree/master/first-network).

*I am a beginner in the field. My sincere apologies to you, in case there are any errors or I failed to explain properly. I am writting so as to get some more experience and clarity over this topic*

### BYFN - Build your First Network example
- **YAML Files**:It all starts with `yaml` files. In case you are wondering what this is, YAML is a language used in most of the configuration files
for many of the applications. Here, we mainly deal with three yaml files
  - **crypto-config.yaml**: Here we define the organizations,number of peers under each organization and the ordering service,which is used for ordering the transactions
  - **configtx.yaml**: Here we include an entry for MSP(Membership Service Providers) for each organization. In addition, we shall also include the names of organizations in the constortium(Collection of non ordering organizations that join the network). Also in the profiles section, along with consortium config you will find channel and orderer configs included. 
  - **docker-compose-cli.yaml**: Required for booting up the entire network. Here we define the containers we requrie - one container for one peer. There is another important container - *cli* container defined here, which provides us command line utlity to start the network, create channels, join orgs to the channel.
  
- **Utilities in action**: Once the configuration is done the utilities use these config files to perform certain tasks like generating certificates, starting a genesis block, creating a channel, joining organizations to the channel.
  - **Cyrptogen**: Crytogen is used in generating Hyperledger Fabric key material. It means it will be used to generate the certificates for the orderers and peers defined in the configuration files.
  Generally the file `byfn.sh` automates most of the commands in setting up your first network. However it is when we look into the file, you understand the process of a network setup
  In `byfn.sh` file, you can find the command 
  ```
  cryptogen generate --config=./crypto-config.yaml
  ```
  cryptogen uses `crypto-config.yaml` to generate the certificates. You will find a new directories created with two sub directories - *peerOrganizations* and *ordererOrganizations*
  The ordererOrganizations directory contains a diretories of each of its orderers. While peerOrganizations contain directories for each of peers. The directory for each peer or orderer includes different directories - `ca`,`msp, `tlsca`, `users`, which contain the necessary key material
  
  - **Configtxgen**: The configtxgen command allows users to create and inspect channel config related artifacts. The content of the generated artifacts is dictated by the contents of `configtx.yaml`(definition in docs, self explainatory)
      - *Creating a genesis block*: The genesis block is the first block in the blockchain. In Fabric, the genesis block contains information about the channel. 
      ```
        configtxgen -profile SampleMultiNodeEtcdRaft -channelID byfn-sys-channel -outputBlock ./channel-artifacts/genesis.block
      ```  
        The above commands creates a genesis block in the `channel-artifacts` folder. `SampleMutliNodeEtcDraft` indicates to generate genesis block for a raft ordering service. `byn-sys-channel`  indicates the name of the channel which is used to indicate by the flag `-channelID`
      - *Creating a channel configuration Transaction*: The channel configuration file `channel.tx` contains a definitions for our sample channel
      ```
      ../bin/configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mysamplechannel
      ```
      (I am deliberately skipping the anchor peers part, as a network can be setup without anchor peers too)
 
 - **Furthur Steps**      
     - *Starting the Network*: `docker-compose`(A tool to run multi container Docker applications) shall be used here to start the containers
      ```
      docker-compose -f docker-compose-cli.yaml -f docker-compose-etcdraft2.yaml up
      ```
      You can check that the containers are running by using the command 
      ```
      docker ps -a
      ```
      - *Creating the channel*: To create a channel we must be able to enter the cli command tool, we do that by running the command
      ```
      docker exec -it cli bash
      ```
      - *Creating  a channel* : Now to create a channel, we pass the channel configuration transaction, channel name, ca-certs to the orderer
      ```
      peer channel create -o orderer.example.com:7050 -c mysamplechannel -f ./channel-artifacts/channel.tx --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
      ```
      - *Join Peers to channel*:
      Set the following as environment variables. This is required so as to join peers to the channel
      ```
      CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
      CORE_PEER_ADDRESS=peer0.org1.example.com:7051
      CORE_PEER_LOCALMSPID="Org1MSP"
      CORE_PEER_TLS_ROOTCERT_FILE=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
      ```
      Use the following command to add peer0 to the channel
      ```
      peer channel join -b mychannel.block
      ```
      Similarly you need to set environment variables for other peers so as to add them,Instead you can write a script like [this](https://gist.githubusercontent.com/ialberquilla/4fd5a94fba3a73e7893b43f3eebca709/raw/a6dc73759b87b544cb1bb850555f8c4f2748faeb/join-peer.sh)
      to add peers to the channel
      
*We are done with setting up a network - a channel with peers joined :)* 
      
