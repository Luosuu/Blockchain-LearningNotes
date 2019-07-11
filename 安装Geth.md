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

## macOS
