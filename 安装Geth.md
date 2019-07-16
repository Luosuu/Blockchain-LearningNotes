# 各种系统安装Geth的办法

## Ubuntu

```shell
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
```

## 禁用了PPA源的Linux发行版

可以考虑编译安装。

```shell
git clone https://github.com/ethereum/go-ethereum
```

安装Go

```shell
sudo apt install golang
```

编译安装

```shell
sudo apt-get install -y build-essential
cd go-ethereum
make geth
```

然后在最后的运行结果里你就可以知道你启动geth的路径，你可以把它加在你的环境变量里方便你使用。

## 如何方便地启动geth

在你安装完geth后，会告诉你geth的安装路径，在shell中直接输入该路径，geth就启动了。

但是我们想在shell中输入geth就能直接启动，而不是每次都要输入长长的路径。

这里介绍一种办法。这个办法就是在用户目录下创建一个叫做`bin`文件夹，然后将其中储存一个软链接，链接到geth的路径。然后在`.zshrc`中添加环境变量，将咱们刚刚创建的`bin`文件夹添加进去。

```shell
# 进入用户家目录，也就是～
cd
# 创建文件夹
mkdir bin
# 创建软链接
ln -s <geth路径> ～/bin/geth
```

至此我们成功地将软链接添加好了。

然后我们添加环境变量。我们应该编辑`.zshrc`文件。在最后添加如下命令：

```shell
export PATH = $PATH:~/bin
```

export是添加环境变量的语句，并且是一次性的。由于我们将它写入的`.zshrc`中，也就是zsh的配置文件，每次我们启动shell的时候都会随zsh的启动而实施。

以上的方法是受推崇的，这样的做法十分安全，避免了sudo。并且保证了每个用户的独立性，也就是每个用户自己软件的不可见性。

如果是个人用户，可以不这么麻烦。可以直接放在`/usr/local/bin`

```shell
sudo ln -s <geth路径> /usr/local/bin/geth
```

这样需要sudo，并且如果是在服务器端，那么所有服务器用户都可以使用。
