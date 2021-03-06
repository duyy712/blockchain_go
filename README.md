# blockchain_go
My simple implement of blockchain with Golang.

Fork from https://github.com/Jeiwan/blockchain_go
(Many thanks to Jeiwan!)

This is part 1 of my articles about my blockchain's implement tutorial below :

1. [Basic prototype](https://github.com/mytv1/blockchain_go/tree/part_1)
2. [Network](https://github.com/mytv1/blockchain_go/tree/part_2)

I'm not good at English. So please tell me if there is something make you hard to understand.

I'm also new in Golang and Blockchain. So if you spot any problem in my code, please feel free to correct it.

# Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Running](#running)
- [Program Structure](#structure)
- [Testing](#testing)
- [References](#references)

# Introduction
In this article, we'll build a simple blockchain with a block structure.
[comment]: <> (Sửa lại câu trên theo đúng ý)
When you run the program, the sample chain of blocks will be printed with its hash and its information.

# Prerequisites
(My local environment)

OS : Ubuntu 16.04.2 LTS
[comment]: <> (Thêm cài đặt go)

```
$ go version
go version go1.9.2 linux/amd64
```
# Running
```
go build .
./blockchain.go
```

# Structure
Basic structure :

### Block
[comment]: <> (Xem xét lại thay đổi thứ tự cho phù hợp :v)
```
type Block struct {
	Timestamp     int64
	Data          []byte
	PrevBlockHash []byte
	Hash          []byte	
}
```

Block is the basic element of blockchain. In our example, I built it with basic properties.
In practice, you can consider ethereum block structure [here](https://github.com/ethereum/go-ethereum/blob/master/core/types/block.go#L139)

In my struct, we have :
- Timestamp : timestamp when a block is created. I use `int64` because this type can be used to present Unix time.

- Data : for simple usage, its type may be `string` or anything else depend on our purposes. But with `[]byte`, we can handle it easily when we need encryption or json marshal.

- Hash : We need a hash to "seal" this block. Hash is special mechanism in blockchain with many purposes : making difficulties to fake an unique block, "proof of work", easy validation... . We will mention it later. In our structure, hash is simple and just `sha256(Data + PrevBlockHash + Timestamp)`. About hash type,  i think `[]byte` is the most suitable, or we can consider `string`.

- PrevBlockHash : Blockchain is a linked list of blocks. So each block will link to previous (created previously) block by saving a hash to it (its pointer in below diagram)
![linked list](https://s3-eu-west-2.amazonaws.com/dotjsonimages/2017/06/ll-4.png)

The first block has no pointer to its previous block, and is called **Genesis block**

Notes: If you are wondering why we shouldn't add NextBlockHash (Seem like it will help us to iterate blocks easier, and blockchain will transform from singly linked list to doubly linked list), that's because of block's stability. As a block view,its previous block is stabler than its next one. With some special conditions, the next block has a higher possibility to be changed than the previous. In my opinion, we should save blockchain's block data as stable as possible.

### Blockchain
```
type Blockchain struct {
	blocks []*Block
}
```

Blockchain structure is just array of Blocks, very simple. And with our purpose, to make a simple blockchain simulation, I think it's enough. Though, we can see at least 2 problems here:
+ Memory storage : Currently bitcoin blockchain size is about 150GB, and we don't want to save something that large to memory. In my opinion, we can save it to files (databases). ~~We will mention it later~~. 
[comment]: <> (Xem câu gạch ngang)
+ PrevBlockHash wasted : With this structure, we can reference to previous block by indexing, therefore block's PrevBlockHash property may be wasted. But I think it's normal, and we can see its uses on future articles.

You may want to consider ethereum blockchain structure [here](https://github.com/ethereum/go-ethereum/blob/master/core/blockchain.go)

# Testing
```
go test
```

Enjoy!

# References
https://jeiwan.cc/posts/building-blockchain-in-go-part-1/


