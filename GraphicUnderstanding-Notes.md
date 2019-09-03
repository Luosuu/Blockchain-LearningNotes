# Notes of Graphic Understanding Etheruem

characterize three main major activities:
1. money transfer
2. smart contract creation
3. smart contract invocation

market capitalization: 20 billion USD

A transaction can be an internal one thar resultes from executing a smart contract due to an external transaction, and therefore an internal transaction's sender is the smart contract.

Note that an external transaction may lead to many internal transcations.

replay all external transactions in a customized Ethereum client.

Various graph analysis on Money Flow Graph, Contract Creation Graph, Contract Invocation Graph:
1. degree distribution
2. clusters
3. degree correlation
4. node importance
5. assortativity
6. strongly/weakly connected component

new approaches based on cross-graph analysis(交叉图分析) to address two security issues:
1. attack forensics(攻击取证) for finding accounts controlled by the attackers
2. anomaly detection(异常检测) for discovering potential attacks through smart contracts.

recent studies of graph-based analysis on Bitcon cannot be applied on Ethereum directly because of the differences in functionalities and protocols(协议)

130 operations for EVM, and bytecode of a smart contract can be considered as a sequence of such operations.

EVM provides 61 handlers to interpreting 61 operations, individually, and 4 special handlers to execute PUSHx, DUPx, SWAPx, LOGx.

five operations can lead to internal transactions:
1. Creat
2. Call
3. Call-Code
4. Delegate-call
5. Self-destruct

CREAT and CAll creat and invoke a smart contract.

CALLCODE and DELEGATECALL also invoke a smart contract, but the callee runs in caller's context.

exclude four types of transactions that are not related to the aforementioned activities:
1. send Ether but amount is 0
2. self-destruct a smart contracts which has no Ether remaining
3. unsuccessful transactions among EOAs because they do not lead to money transfer
4. unsuccessful transactions for smart contract creation.

Since the code of the construction functiom will be discarded after smart contract creation and thus no users can invoke it again, the transactions for smart contracts creation are not considered when building CIG.

Only 0.8% of EOAs do not transfer Ether.

more than 2/3 smart contracts do not transfer Ether.

81% of accounts(96% sc and 77% EOAs) are involved in no more than 5 transactions which means that most accounts are infrequnet in transferring money

the proportion of developers is just 1% of total users.

99.5% of sc are involved in only 1 transaction. Hence, almost all contracts do not creat contracts possiblly beacuse developers rarely exploit this advanced funtionality.

73% EOASs do not invoke sc and 81% sc are not invoked.

96% EOAs call sc no more than 5 times.


## Construction

### MFG 

MFG = (V,E,w)

V is a set of nodes

E is a set of ordered pairs of nodes, where the order of an edge indicates the direction of transferred money.

w is a function mapping edges to their weights.

### CCG 

CCG = (V,E)

V is a set of nodes, same as MFG

E is a set of ordered edges, in which $(V_i,V_j)$ which means $V_i$ created $V_j$.

#### Properties of CCG

A forest consisting of multiple tress.

The root of each tree is an EOA, the other nodes of the tree are smart contracts directly or indirectly created by the root.

Smart contracts obviously outnumber the EOAs which creat contracts.

### CIG

CIG = (V,E,w)

w is a function which associates each edge with a weight, which is the total number of invokations along the edge by one or more transactions.

### Insights

sc not widely used

not all users frequently ues Ethereum

Users prefer to transferring money instead of using smart contracts

indegree(入度)

outdegree(出度)

## Analysis

global clustering coefficent(全局聚类系数) to evaluate the extent(程度) to which nodes in a graph tend to cluster together.

Pearson coefficent(皮尔逊相关系数，统计学名词) to evaluate the correlation(相关度) between the indegree and the outdegree of nodes, compute the assortativity coefficent(相似性系数) to study the preference for nodes to attach to others, and evaluate node's importance using the PageRank algorithm.

Exchange markets are the hub nodes(枢纽节点) connecting yo other nodes bidirectionally(双向), resulting in the huge SCC.

*ReplaySafeSplit* is an important sc and used to prevent the attacks that replay transactions between the old chain and new forked chain.

