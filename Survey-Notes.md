# "A Survey on the  Security of Blockchain systems" Notes

All the content are from:

Li, X., Jiang, P., Chen, T., Luo, X., & Wen, Q. (2017). A survey on the security of blockchain systems. Future Generation Computer Systems.

## Risks

### 51% Vulnerability

example:

gash.io reached 42% of total Bitcoin computing power.

### Private Key Security

Hartwig discover a vulnerability in ECDSA scheme, through which an attacker can recover the user's private key because it does not generate enough randomness during the signature process.

### Criminal Activity

### Double Spending

An attacker could leverage race attack for double spending. This kind of attack is relatively easy to implement in PoW-based blockchains, because the attacker can exploit the intermediate time between two transactions’ initiation and confirmation to quickly launch an attack. Before the second transaction is mined to be invalid, the attacker has already got the first transaction’s output, resulting in double spending.

### Transaction Privacy Leakage

The privacy protection measures in blockchain are not very robust.

66.09% of all transactions do not contain any mixins. Since users may use the outputs of 0-mixin transaction as mixins, these mixins will be deducible.

Moreover, they study the sampling method of mixiins and find that the selection of mixiins is not really random. Newer TXOs(transaction outputs) tend to be used more frequently.

They further discover that 62.32% pf transaction inputs with mixins are deducible.

By exploiting these weaknesses in Monero, they can infer the actual transaction inputs with 80% accuracy. 

## Specific Risks to Blockchain 2.0

### Criminal Smart Contracts(CSCs)

An example of password theft CSC PwdTheft.

### Vulnerabilities in Smart Conrtract

Vulnerabilities from:
1. Contract sourcr code
2. EVM bytecode
3. Blockchain mechanism

bugs are as follows:
* Transaction-ordering dependence
* Timestamp dependence
* Mishandled exceptions (错误处理的异常)
* Reentrancy vulnerability（可重入漏洞）

### Under-Optimized Smart Contract

(欠优化的智能合约)

1. Dead code
 * It means that some operations in a smart contract will never be executed,but they will still be deployed into the blockchain.  Since in the smart contract deploymentprocess the consumption of gas is related to bytecode size, the dead code will cause additionalgas consumption
2. Opaque predicate 
 * For some statements in a smart contract, their execution resultsare  always  the  same  and  will  not  a ect  other  statements  and  the  smart  contract.   Thepresence  of  the  opaque  predicate  causes  the  EVM  to  execute  useless  operations,  therebyconsuming additional gas.
3. Expensive operations in a loop

### Under-Priced Operations

It's hard to accurately measure the consumption ofcomputing resources of an indicidual operation, and therefore some gas values are not set properly.

Some IO-heavy operations'gas values are set too low, and hence these operations can be executed in quantity in onetransaction.   In  this  way,  an  attacker  can  initiate  a  DoS  (Denial  of  Service)  attack  onEthereum.

under-priced operation EXTCODESIZE and SUICIDE.

In order to solve the security problem caused by under-priced operations, the gas values of 7 IO-heavy operations ae modified in EIP150(Ethereum Improvement Proposal).

Note that EIP150 has already been implemented in the Ethereum publicchain by hard fork, and the new gas table parameters are used after No.2463000 block.

## Attack Cases

Some real attacks on blockchain systems.

### Selfish Mining Attack

Only in the situation where the computing power of the selfish miners are more than that of the honest miners.

Selfish mining attack is conducted by attackers for the pirpose of obtaining undue rewards or wasting the computing power of honest miners.

To conclude, the selfish miners holds discovered blocks privately and then attemps to fork a private chain. Afterwards, selfish miners would mine on this private chain, and try to maintain a longer orivate branch than the public branch. New blocks mined by the honest miners would be revealed when the public banch approaches the length of private branch, such that the honest miners end up wasting computing power and gaining no reward.

### DAO Attack

DAO is a smart contract deployed in Ethereum, which implements a crowd-funding platform and has raise 150 million US\$, which is the biggest crowdfund ever. The attacker stole about 60 million US\$.

The attacker publishes a malicious smart contract, which includes a withdraw() function call to DAO in its callback function. The withdraw() will send Ether to the callee, which is also in the form of call. Therefore, it will invoke the callback function of malicious smart contract again. In this way, the attacker is able to steal all the Ether from DAO.

### BGP Hijacking Attack

To intercept the network traffic of blockchain,attackers either leverage or manipulate BGP routing. 

BGP hijacking typically requires the control of network operators, which could potentially be exploited to delay network messages.

Because of the high centralization of some Bitcoin mining pools, if they are attacked by BGP hijacking,it will have a signficant effect. The attackers can effectively split the Bitcoin network, or delay the speed of block propagation.

Attackers conduct BGP hijacking to intercept Bitcoin miners' connections to a mining pool server.

By rerouting traffic to a miningpool  controlled by the attacker,  it  was possible to steal cryptocurrency from the victim.

### Eclipse Attack

The eclipse  attack allows an attacker to monopolize all of the victim's incoming and outgoing connections, whic isolates the victim from the other peers in the network.

Furthermore, the attacker is able to leverage the victim's computing power to conduct its own malicious acts.

There are two types of eclipse attack considered, namely botnet attack and infrastructure attack.

The botnet attack is launched by bots with diverse IP address ranges.

The infrastructure attacks models the threat from an ISP(Internet Service Provider), company or nation-state that has contiguous IP address.

The Bitcoin network might suffer from disruption and a victim's view of the blockchain will be filtered due to the eclipse attack.

Some other attacks may be caused by the eclipse attack, such as engineering block races, spliting mining power, selfish mining, 0-confirmation double spend, N-confirmation double spend.

### Liveness Attack

Liveness attack is able to delay as much as possible the confirmation time of a target transaction.

Liveness attack consists of three phases, namely attackpreparation phase, transaction denial phase, and blockchain retarder phase.

### Balance Attack

Blance Attacks is against PoW-based blockchain, which allows a low-mining-power attacker to momently disrupt communications between subgroups with similar mining power.(允许低采矿率攻击者立即破坏具有相似采矿能力的子群之间的通信)

They abstract blockchain into a DAG(Directed Acyclic Graph) tree.(有向无环树)

After introducing a delay between correct sub-groups of equivalent mining power, the attacker issues transactions in one subgroup (called "transaction subgroup") and mines blocks in another subgrou (called "block subgroup"), toguarantee that the tree of block subgroup outweighs the tree of transaction subgroup.

Eventhough the transactions are committed, the attacker is able to outweigh the tree containingthis transaction and rewrite blocks with high probability.

## Security Enhancements

### SmartPool

SmartPool gets the transactions from Ethereum node clients, which contain mining tasks information. Then, the miner conducts hashing computation based on the tasks and returns the completed shares to the smartpool client.

When the number of thecompleted shares reaches to a certain amount, they will be committed to smartpool contract,which is deployed in Ethereum. The smartpool contract will verify the shares and deliverrewards to the client. Compared with the traditional P2P pool,SmartPoolsystem hasthe following advantages:

1. Decentralized
2. Efficency
3. Secure
 * SmartPool leverages a novel data structure, which can prevent the attacker from resubmitting shares in different batches. Furthermore, the verification method of SmartPool can guarantee that honest miners will gain expected rewards even there exists malicious miners in the pool.

### Quantitative Framework

(量化框架)

There exists tradeoffs between blockchain's performance and security.

A quantitative framework which is leveraged to analyze PoW-based blockchain's execution performance and security provisions.

The framework has two components: blockchain stimulator and security model.

The simulator mimics blockchain's execution, whose inputs are parameters of consensus protocol and network.(其参数是共识协议和网络的参数)

Through thesimulator's analysis,  it can gain performance statistics of the target blockchain, includingblock propagation times, block sizes, network delays, stale block rate, throughput, etc. 

The stale block refers to a block that is mined but not written to the public chain. The throughput is the number of transactions that the blockchain can handle per second. Stale block rate will be passed as a parameter to the security model component, which is based on MDP(Markov Decision Processes)(马尔可夫决策过程)for defeating double spending and selfish mining attacks.

The framework eventually outputs optimal adversarial strategy against attacks, and facilitates building security provisions for the blockchain.

### OYENTE

OYENTE is proposed to detect bugs in Ethereum smart contracts.

OYENTE follows the execution model of EVM and leverages symbolic execution to analyze the bytecode of smart contracts.

### Hawk

Hawk is a framework for developing privacy-preserving smart contracts.

### Town Crier

TC(Town Crier) is an authenticated data feed system for data interaction process with off-chain(i.e., external) data source.

Smart contract deployed in Ethereum cannot access netwrok directly, they cannot get data through HTTPS.

TC acts as a bridge between HTTPS-enabled data source and smart contracts.

TC contract is the front end of the TC system, which acts as API between users' contracts and TC server.

The core program of TC is running in Intel SGX enclave. The main function of TC server is to obtain the data requests from users' contract, and obtain the data from target HTTPS-enabled websites. Finally, the TC server will return a datagram to the users' contracts in the form of digitally signed blockchain messages.

Relay module is designed as a network communicationhub for smart contracts, SGX enclave environment, and data source websites. Therefore, it achieves isolation between network communication and the execution of TC's core program.Even if the Relay module is attacked, or the network communication packets are tampered, it will not change the normal function of TC.