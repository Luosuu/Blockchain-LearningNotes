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


