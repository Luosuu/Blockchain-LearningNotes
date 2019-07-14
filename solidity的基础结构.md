# solidity的简单入门

Solidity是针对以太坊的编程语言，受到C++，Python，JavaScript等语言的影响。它的设计目的就是为了在以太坊上运行。

它是一种静态语言。在[智能合约的简单理解](https://github.com/Luosuu/Blockchain-LearningNotes/blob/master/%E6%99%BA%E8%83%BD%E5%90%88%E7%BA%A6%E7%90%86%E8%A7%A3.md)中的代码里我们可以窥见到，solidity支持一部分类的特性。solidity同样支持继承，和复杂的由用户自定义的特殊类型。因此我们可以说solidity是一种OOP（面对对象）的语言。我们在编写solidity代码时，应该以面对对象的编程方式定义变量（状态）和函数。并在solidity中，是大小写敏感的。

我们知道以太坊上的每个节点实际上一般都是EVM，就算是矿工节点，一般也承担EVM的职责。以太坊区块链通过编写和执行智能合约来帮助拓展它的功能。EVM支持的语言很多，solidity是其中最流行也是最适合的。

## EVM和solidity

EVM是最终执行智能合约代码的地方，但是它不能直接理解solidity里面的高级的结构。EVM能理解的是一种被称为**字节码**的一种低级指令。这种指令非常精简。深入了解EVM，你会发现EVM在实际运行代码时不能联网，权限也非常有限。包括使用精简的指令在内的这些特性，都是为了保证EVM的安全性。

要把我们编写的solidity代码转换成字节码，需要编译器。solidity附带的编译器成为solidity编译器或者**solc**。

那么这个工作流程就和普通的编译代码并运行它没有什么太大的区别了。

solidity代码 -> solc -> 字节码 -> 部署并在EVM上运行。

## solidity文件

储存solidity代码的solidity文件的拓展名是`.sol`。solidity文件是人类可读的文件，可以在任何编辑器，甚至可以在记事本里打开。

solidity文件由以下四个高级结构组成

* 预编译指令 - pragma
* 注释
* 导入(import)
* 合约/库/接口 - contract/library/interface

其中solidity的注释是和C++是一样的，`\\`是单行注释，`\*   *\`是多行注释。

它们整体看起来像是这个样子的

```solidity
pragma solidity 0.4.19;

contarct a{
    //***
}
library b{
    //***
}
interface c{
    //***
}
```

### import语句

帮助我们导入其他solidity文件，使得当前的solidity文件可以访问其中的代码。这有助于我们编写模块化的代码。如：

```solidity
import 'commonLibrary.sol';
```

### 预编译指令

通常指solidity文件的第一行代码，其形式一般为

```solidity
pragma solidity <<version number>>;
```

注意是有分号作为结束的。

在`pragma`指令的帮助下，你可以为你的代码选择合适的编译器。这是一种很好的习惯。

其中版本号`^0.4.0`代表版本号为4的最新的版本。
