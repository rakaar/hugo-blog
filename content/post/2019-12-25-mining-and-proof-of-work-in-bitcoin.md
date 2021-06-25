---
date: "2019-12-25T00:00:00Z"
subtitle: Understanding the mining details of bitcoin
tags:
- bitcoin
- mining
- PoW
- consensus_protochol
- blockchain
title: Mining and Proof of Work in Bitcoin
---

# Introduction
Pre-requisite: Basic understanding of Blockchain.
More specifically, I will be talking about Bitcoin's mining. But it also applies to other cryptocurrencies following the same consensus protocol(we will see what that exactly means). My aim, while writing this is to provide some intuition behind proof of work algorithm and to explain in detail behind the puzzle that is solved by the miners in order to add a valid block to the Blockchain. I will be focussing on the parts which are not very abundant on the internet or are compromised with a high-level overview of things.
# Intuition behind consensus protocols and Proof of work
Bitcoin or in fact any other cryptocurrency is decentralized. That means there is **NO** central authority to provide services, maintain records, etc. That is the main aim of introducing Bitcoin - Having no central Master. In our daily life, it is the banks that send money on our behalf, keep a track of how much money each individual is left with. Now in case of a decentralized system, how do you achieve that? 
The set of rules by which a decentralized system comes to an agreement that 'Yes! this is the state of the system.'.  For example in  Bitcoin, everyone needs to come to the conclusion that a particular transaction is valid and can be appended to the Blockchain. 
Now for Bitcoin, let's think of a naive Consensus protocol, where each member in the network provides one vote. Seems fair? No!
Well in Bitcoin, to become a part of a network all you need is a device and internet connection. A malicious individual(say Mr.X) can easily take advantage of this. X can now create multiple identities with ease. He will create a fake transaction and broadcast it to the network. Since many of the nodes in the network are controlled by X, they will receive the majority votes and hence will be validated. Likewise, there might be many people like X in the network. The whole system is flawed :(
So what can be done?  The basic step towards solving this problem is to make the process of voting a bit difficult. Or to rephrase it- Make a vote costlier. That means, for an individual in the network to vote whether the transaction is valid or not he or she also has to spend some resources.  Resources that are valuable, available with everyone so that everyone has an equal opportunity to participate. Satoshi Nakamoto(inventor of Bitcoin) chose "Computing Power". In order to validate a transaction, you need to spend some amount of computing power. You need to provide proof that you have spent your resources to validate the transactions and add them to the blockchain. This proof is provided by solving a cryptographic hash puzzle(we will see in detail about it) which is computationally difficult to solve since the only way to get the right answer is by trial and error.  This consensus Protocol is named as *Proof of Work*.
In simpler words, Proof of work is a method by which you translate your computing power to voting power. That is also why, in bitcoin white paper you can see the words `1 CPU 1 Vote`, rather than `1 Identity 1 Vote`, which is a general case.
What is the Cryptographic Hash puzzle? How is it solved? To understand that, we need to know about the anatomy of a Block in Bitcoin Blockchain.

# Anatomy of a block
A block in a blockchain consists of these following things(* means will explain soon)
*  Block header: Consists of Metadata of the block, which enforces the security of the blockchain
    * Previous block hash: Evident from the name,  Hash of the Previous Block
    * Merkle Root*
    * Nonce *
    * Timestamp: Evident from the name, time at which the block was created.
    * Version:  Version of the Bitcoin software used by the miner
    * Block hash*
    * Bits*
* Blocksize:  Size of the block
* Transaction Counter: Number of transactions in a block
* Transactions: The actual list of transactions
# Merkle Root:
It is the hash of the transactions in a tree fashion. In simpler words, Merkle root is the summary hash of all the transactions of a given block. Check the below image for clarity.
![Merkle Root](/img/mining-merkle-root.jpg)
Why do you have to do that? Why not just hash all of them Linearly at once?
The main reason behind this is that it will be computationally cheaper to verify whether a single transaction is present or not in this case.  
Suppose there are 8 transactions, as per the below image and we hash all of them linearly to obtain the summary, to verify any transaction we need all the other 7 transactions, Check the image below
![Non Merkle Root 7 are required](/img/mining-merkle-7.jpg)
In case of a tree, you need just three hashes that are marked in blue to verify whether that particular transaction exists or not
![Merkle only 3 are required](/img/mining-merkle-3.jpg)
BlockHash:
Block hash is obtained by hashing these three things - Previous Blockhash, Merkle root, and Nonce. 
Nonce:
A nonce is a number that is hashed with Previous block hash and Merkle root to find a valid block hash. When do you consider a block hash valid? A block hash is considered valid when it is less than a specific number called `target` which is embedded in Bits of the block.
Bits:
As mentioned previously, it is a compact notation of the target. For example, taken from [this](http://blog.geveo.com/Blockchain-Mining-Difficulty) article
Bits of a certain block is 402809567 in decimal when converted to hex: 18 02 62 DF. 
Now the first byte indicates the length of the hash target. So in this case,  24 (18 in hex) would be the length of the hash target. And the next 3 bytes are the most significant digits of the hash target. Hence the hash target would be 
as per this image
# The cryptographic Hash puzzle
Now let us understand the puzzle that is solved to add a block to the blockchain. It is here that you get a clear understanding of the terms - Nonce, and Bits. 
A miner's job is to find a number that when hashed with the Previous Block hash and Merkle root gives the hash which is less than the hash target of the block.
In simpler words 
hash(prevBlockhash, MerkleRoot, Num) < HashTarget
Our aim is to find the Num 
The starting value of Num is 0, they hash it and check if the hash is less than HashTarget or not. If not increment it and hash it again and check again if it is less than the hash target or not. And the process is continued until a valid block hash is found, that means finally the resulting hash is less than the HashTarget.
The Num is called Nonce because it is used only once to check, if it doesn't meet our requirements, then we increment and continue checking. But we are not going to use the same number again with a particular pair of Previous Blockhash and Merkle Root.
By solving the cryptographic hash puzzle to validate transactions means to find a number when hashed with previous block hash and the summary hash of transactions(Merkle root) results in a number less than the hash target. This is what it all boils down to!
How tough is this?
Just for sake of analogy, imagine if you are blindfolded and you are asked to hit only a specific region of the dartboard. How can you achieve it? The only way is by throwing more darts on the board hoping that they would hit the specific region. Similarly here, the only way is to increase the probability of success is by checking with more numbers(Nonces). 
Now you can see that it takes a decently large amount of computing power to add a block to the blockchain, hence validate the list of transactions. Hence, the miner who has solved the puzzle has a 'proof of work' done by finding the valid block hash, which we have seen how computationally expensive is this.
### What is the maximum value of the nonce? Does it keeping going on and on and on.....?
No! The nonce is a 32-byte value. So its maximum value is 2^32 - 1. What if no value of nonce satisfies the given condition. That is no number less than equal to 2^32-1 when hashed with the previous block hash and Merkle root of the block results in hash less than the hash target?
Do we change the hash target?
No!
Do we change the previous block hash?
Obviously No! How can one do that?
The left thing is Merkle Root. How do you change that?
We have missed one fact! Let us see that here:

# Coinbase Nonce
When a miner adds a valid block to the blockchain, he receives a fixed reward. Even that is considered as one of the transactions in the block, that has a special name called 'Coinbase Transaction'. At the lowest level in the Merkle tree, the coin base transaction is the first transaction. There is something called  'Coinbase Nonce', which is just arbitrary data that is hashed in the process of creating a Merkle root. Now to change the Merkle root, since we can't change the rest of the transactions. It is this Coinbase Nonce that can be changed and hence changing the Merkle Root.
Now you can start the nonce value fresh from 0 again and check by hashing with the previous Blockhash and this new Merkle Root verify. Increment the nonce and repeat the process.  If that fails too, try again with a different Coinbase nonce, hence a different Merkle Root
# Reward for miners
There is a reward for miners for every valid block they add to the blockchain. Generally, this reward is decreased by 1/2 for every 210,000 blocks. The initial block rewards from the beginning were 50 BTC.  The current Blockreward is 12.5 BTC. 210,000 blocks are approximately mined for a period of 4 years. (You will see soon how these are approximated). There is one more interesting thing to note. Mining is the only process through which Bitcoins can be generated. So, one can conclude that the maximum number of Bitcoins that can exist are:

```210,000(50 + 25 + 12.5  + 6.25 + ....) = 21,000,000 BTC```

21 Million is the maximum number of bitcoin that can be created. 
# Is the Hash target fixed?
No dear!  There are times where there are more miners in the network, and times when there are very few in the network. The main goal of Satoshi Nakamoto was to keep the block time fixed as approximately 10 minutes. That is for every 10 minutes, a block is added to the blockchain. 
The way to keep this time constant is to adjust the Hash target. Suppose there are many miners in the network, and the blocks are getting mined soon. We need to increase the difficulty of the puzzle - that implies we need to reduce the Hash target. Why?  Because there are fewer choices. For example, if the hash target was 0FFFF (65,535), which means the miner needs to find a nonce which when hashed with previous Block hash and Merkle Root gives a number less than 65,535. We have 65,536 choices(0 to 65,535). Too many! Now if the hash target was 000FF(255), we have only 256 choices. 
And if there are less number of miners, it means the blocks are mined slowly, hence we need to make the puzzle easier, Hence increase the Hash target. In analogy with the Darts game, think of it as adjust the radius of the region where you want the dart to get hit.
And this is changed on observing the average block time of the last 2016 blocks(approximately every 2 weeks, why?
2016 x 10 mins = 20160 mins  = 14 days = 2 Weeks).
To quantify the above words:
![Hash Target](/img/mining-hash-target.png)
*Image Credits from [this blog](http://blog.geveo.com/Blockchain-Mining-Difficulty)*

# Final words
Currently, Most of the mining in bitcoin is not done individually. People who mine come together and form Mining pools. Here, under the guidance of a pool manager, profits are distributed accordingly as per various pool schemes. 
This is just one of the many interesting aspects of Bitcoin. Finally, I would like to conclude with this beautiful tweet!
![Tweet](/img/mining-final-tweet.png)