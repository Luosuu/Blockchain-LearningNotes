# Geth搭建私链

## 创建目录

首先我们需要创建一个我们用于储存私链数据的文件夹，geth客户端工作的时候导入数据就要从其中导入。

```shell
mkdir private-geth
cd private-geth
touch gensis.json
```

## 创世区块

我们新建的`gensis.json`文件是创世区块的配置文件。

这个文件会保证没有其他节点和你的节点的区块链版本一致，除非他们的创世区块的配置文件和你一模一样。

每条链都应该有创世区块，也自然应该有自己的`gensis.json`

以下是一个例子，你需要在了解其中的各项参数的意义后自己更改一下。

```json
{
"config":{
"chainId":39,
"homesteadBlock":0,
"eip155Block":0,
"eip158Block":0
},
  "alloc"      :{
      "0xeb680f30715f347d4eb5cd03ac5eced297ac5046":{"balance":"10000000000000000"}},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0xffffffff",
  "nonce"      : "0x0000000000000039",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}
```

其中`alloc`代表初始账号，并且分配给他一定的余额。

建议这里设置一个初始账号，方便我们以后的操作。账号可以自己随便写，只要格式正确，长度正确就可以，因为我们不会实际使用这个账号。

## 初始化

然后我们需要使用创世区块的配置文件初始化我们的私链。首先我们需要一个储存链数据的位置。

我们推荐在我们刚才创建的文件夹里在创建一个文件夹。

```shell
# 名字不重要
mkdir db
```

然后我们需要执行初始化命令，进入我们最开始创建的文件夹里。

```shell
geth --datadir "./db" init gensis.json
```

`--datadir "./db"`代表指定存储位置，后面的代表将`gensis.json`作为初始化配置文件。

然后你可以进去使用ls命令看看里面都有些什么。

## 启动节点

进入我们最开始创建的文件夹，使用以下命令启动节点。

```shell
geth --datadir "./db" --rpc --rpcaddr=0.0.0.0 --rpcport 8545 --rpccorsdomain "*" --rpcapi "eth,net,web3,personal,admin,shh,txpool,debug,miner" --nodiscover --maxpeers 30 --networkid 3909 --port 30303 --mine --minerthreads 1 --etherbase "0xeb680f30715f347d4eb5cd03ac5eced297ac5046" --allow-insecure-unlock console
```

其中`--allow-insecure-unlock`我推荐加上去，否则会在后面的解锁账户时遇到麻烦，**现在不允许以默认的方式解锁带http接口的账户了**。

`-rpc`就是开启了HTTP-RPC服务。

启动节点后他就会开始挖矿。挖矿账户就是我们在`gensis.json`里设置的账户。

最后我们如果想进一步操作，需要进入Geth的JavaScript控制台。

我们需要通过attach命令，连接一个已经启动的节点，这里推荐新开一个终端窗口。

```shell
geth --datadir './db' attach ipc:./db/geth.ipc
```

## 以太坊JavaScript控制台命令

以太坊JavaScript控制台中内置了一些对象，方便我们和以太坊交互，其中有eth, net, admin, miner, personal, txpool, web3。

### 新建账号

```JavaScript
personal.newAccount("123456")
```

括号里的字符串是你新建账号的密码。显示出的结果就是账号的公钥。

生成的账户会保存在keystore文件夹。

你可以新建两个账号，用于接下来我们尝试进行交易。

新建完账号后，可以使用如下命令来查看当前链的账户。

```JavaScript
eth.accounts
```

输出结果就是当前链的所有账户了。他们以数组的形式储存，所以可以用诸如`eth.accounts[0]`的形式调用它们。

然后我们可以看看他们的余额。

```JS
balance = web3.fromWei(eth.getBalance(eth.accounts[0]),"ether")
```

`eth.getBlance`返回的是以wei为单位的余额，上面的命令，将wei单位转化为了ether单位。

新建的两个账户的余额自然是0，想要实现交易至少得先有以太币，所以我们需要用一个账户挖矿，获得奖励。

### 挖矿

首先设立本机挖矿奖励地址。

```JS
miner.setEtherbase(eth.accounts[0])
```

然后检查一下是否设置成功

```JS
eth.coinbase
```

上面这条命令就会返回挖矿的奖励地址。

然后启动挖矿。

```JS
miner.start(1)
```

括号里的数字代表线程数，代表开启几个线程进行挖矿。

你可能遇到返回是`null`的情况，你可以检查下区块高度

```JS
eth.blockNumber
```

会返回一个数字，代表着现在的区块高度，你隔段时间检查一下，如果区块高度增高了，说明它已经开始挖矿了，没有问题。

据我粗略观察，应该是咱们开了一个新的窗口，挖矿的具体信息都在原先的窗口里，如果你担心区块高度的增加是因为原先的挖矿没有停止，你可以先用`miner.stop()`，再用`miner.start(1)`,然后检查区块高度是否增加

### 交易

进行一段时间的挖矿后，你说设置的奖励账户里的余额应该增加了。

然后就可以进行交易的准备。

首先我们需要解锁账户，才能实施交易。之前在启动geth时添加的参数中，`--allow-insecure-unlock`就是为了方便这一步的。

解锁命令为：

```JS
web3.personal.unlockAccount(web3.personal.listAccounts[0],"<password>", 15000)
```

最后的数字代表解锁的时间，单位为秒。`<password>`代表你要解锁的账户的密码。

虽然有另外一种解锁的方式，但是我没有成功。

```JS
personal.unlockAccount(eth.accounts[0],"<password>",15000)
```

然后就可以交易啦。

```JS
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value:web3.toWei(1,"ether")})
```

