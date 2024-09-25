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



