# Black-Box IoT: Authentication and Distributed Storage of IoT Data from Constrained Sensors
## What is it?
It is a simplified, blockchain-based black box system for IoT devices, who have limited computation and storage resource, to store data and do authentication immutably.  
The challenge to use blockchain is the dependency on asymmetric authentication techniques, which is expensive. So instead, this work introduces a novel hash-based digital signature that uses an onetime hash chain of signing keys, without synchronization and timing.
## Background
### Blockchain
**Permissioned vs Permissionless** About which kind of nodes can participate as maintainers. The former means central authority is needed (Hyperledger Fabric). The latter means everyone could join, with PoW or PoS (Bitcoin, Ethereum).  
Although the nature of permissionless blockchain is attractive, it is not needed for IoT.
## Hyperledger
- Clients are responsible for creating a transaction and submitting it to the peers for signing.
- Peers are the blockchain maintainers, and are also responsible for endorsing (authenticating messages) clients’ transactions.
- Orderers after receiving signed transactions from the clients, establish consensus on total order of a collected transaction set, deliver blocks to the peers, and ensure the consistency and liveness properties of the system.
- The Membership Service Provider (MSP) is responsible for granting participation privileges in the system.  
![bbox-modified-hyberledger-fabric](https://github.com/vinland-avalon/Readings/blob/main/images/bbox-modified-hyberledger-fabric.png?raw=true)
## Hash-based signature
Inspired but different from Lamport passwords [40] and TESLA [49, 50], it avoids the need for any synchronization between senders and receivers. It only need to do hash operations.  
### The algorithm
First, it is a chain-based one-time signature scheme, with each key derived from its predecessor as 𝑘i ←h(𝑘i+1 ),𝑖 ∈ {𝑛−1,𝑛−2,...,0}. (𝑘𝑖 , 𝑘𝑖 −1 ) can be viewed as a public-private key pair and the key 𝑘 serves as the “private seed" for the entire key chain.  
For example as shown in Figure 2, we can construct a hash chain from seed 𝑘5. For signing the 1st message 𝑚1, the signer would use (pk1 ,sk1 )=(𝑘0 ,𝑘1 ) and output signature 𝜎=h(𝑚1 ||𝑘0 )||𝑘1 . Similarly, for the 2nd message he would use (pk2 , sk2 ) = (𝑘1 , 𝑘2 ) and for the 5th message(pk5 ,sk5 )=(𝑘4 ,𝑘5 ).  
![bbox-iot-hash-based-signature](https://github.com/vinland-avalon/Readings/blob/main/images/bbox-iot-hash-based-signature.png?raw=true)
It is susceptible to Man-in-the-Middle attacks. However, there are other mechanisms in this paper to prevent it.

