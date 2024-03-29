---
title: "Blockchain fundamental"
layout: "blog"
tags: ["blockchain"]
series: ["blockchain"]
---


# 区块链技术与应用

## 1 引言

### 1.1 课程基本信息

参考资料：

​	以太坊白皮书、黄皮书、源代码

​	solidity

## 2 比特币 

### 2.1 BTC密码学原理

crypto-currency 

哈希

cryptographic hash function

+ collision resistance （x!=y, but H(x)==H(y)）—— collision free

​		例：MD5 已知人为制造哈希碰撞

+ hiding  x-->H(x) 单向   各种取值差不多

​		结合实现 --> digital commitment / digital  equivalent of a sealed envelope

​		sealed envelope：预测结果会影响结果--不提前公开--不能篡改（公布H(x)，验证x，实际x||nonce）

+ puzzle friendly:不知道哪个输入会出现某种结果

  H(block header) <= target     ==> proof of work

  difficult to solve, but easy to verify

SHA-256 (secure hash algorithm)

签名（账户）

+ (public key, private key)  asymmetric encryption algorithm

  ( 加密(签名), 解密（验证)) encryption key

  产生公私钥 a good source of randomness

### 2.2 BTC数据结构

+ hash pointers( pointer -> address + value)

  ​	block chain is a linked list using hash pointers

  ​	genesis block <- xxx...xxx <- most recent block (保存最后 tamper-evident log)

+ Merkle tree

  ​	类似binary  tree，但最下层data blocks(tx) 上面都是hash pointers

  ​	tx: block header(轻节点) / block body

  ​	提供Merkle proof: 寻根路径 Hash值验证 -->轻节点 

  ​	proof of membership/inclusion(O(log())  \ non-membership (O(n)) hash排序后(sorted Merkle tree)->log(n)

### 2.3BTC协议

​	数字货币问题 double spending attack 

​	中心化 -->f共同维护 区块链

​	一个交易：输入：币的来源/A的公钥 | 输出：目标公钥hash地址

​		例子：A-->B

| block header                  | block body       |
| ----------------------------- | ---------------- |
| version                       | transaction list |
| hash of previous block header |                  |
| Merkle root hash              |                  |
| target                        |                  |
| nonce                         |                  |

full node(fully validating node)   |  light node

+ distributed consensus

  ​	distributed hash table

  ​	FLP impossibility result --- asynchronous faulty | CAP theorem

  ​	常见协议：Paxos 

+ Consensus in Bitcoin（共识机制）

  ​	若基于投票的方法：membership (hyperledger fabric) -- sybil attack

  ​	longest valid chain -- forking attack  等长时维持直至新的出现

  ​	产生新的币coin-base transaction -- block reward

  ​	共识--账本--记账权

  ​	防范sybil attack -- 算力--mining

### 2.4 BTC系统实现

+ transaction-based ledger

  ​	UTXO: Unspent Transaction Output  (total inputs == total outputs)	

  ​	transaction fee: 交易费

+ account-based ledger(ETH)

+ transaction

  ​	nonce 64  + coin-base 32 ==>96

  ​	fees = total input -  total output

+ 挖矿过程分析

  + 每次可以看做是Bernoulli trail: a random experiment with binary outcome

    构成一个Bernoulli process: a sequence of independent Bernoulli trials

    性质：memoryless 前一次对后一次无关  ==>progress free公平性保证

    Poisson process  exponential distribution 整个系统平均出块时间10分钟

  + geometric series

    等比数列 总计2100万

  + 防范double spend 多等几个（6个）confirmation

zero confirmation

selfish mining攻击：folking attack | 屯一个减少竞争(风险)

### 2.5 BTC网络

The Bitcoin Network:

+  application layer: Bitcoin Blockchain

   network layer: P2P Overlay Network 

+ 原则：simple, robust, but not efficient

  ​		flooding

+ 每个节点维护交易集合

+ 区块大小<=1M

+ best effort

### 2.6 BTC挖矿难度

+ 核心：H(block header) <= target

SHA-256
$$
difficulty = difficulty\_1\_target \over target
$$
难度 | 算力 | 出块时间

​	当出块时间减少 分叉过多 51% attack

​	ghost -- orphan block -- uncle reward

+ 每2016个区块（约14天）调整一次

  调整公式：

$$
target = target \times {actual\ time \over expected\ time}
$$

$$ { next\\_difficult}
next\_difficult
$$

actual time: time spent mining the last 2016 blocks

​	最多4倍



### 2.7 BTC挖矿

**复习**

+ 全节点
  + 一直在线
  + 本地硬盘上维护完整的区块链信息
  + 内存里维护UTXO集合，以便快速检验交易的正确性
  + 监听比特币网络上的交易信息，验证每个交易的合法性
  + 决定哪些交易会被打包到区块里
  + 监听别的矿工挖出来的区块，验证合法性
  + 挖矿： 决定沿哪一条链挖 | 当出现等长分叉时，选择哪一个

+ 轻节点（大部分）
  + 不是一直在线
  + 不用保存整个区块链，只要保存每个区块的块头
  + 不用保存全部交易，只保存于自己相关的交易
  + 无法检验大多数交易的合法性，只检验与自己相关的那些交易的合法性
  + 无法检测网上发布的区块的正确性
  + 可以验证挖矿难度
  + 只能检测哪个是最长链，不知道那个是最长合法链

**挖矿**

+ 挖矿设备

  - CPU->GPU->ASIC(application specific integrated circut, 某种mining puzzle)

+ 矿池

  - pool manager ---- 很多miner

  - 工作量证明 收益分配

  - 如果矿池占到50%+算力：folking attack(分叉) boycott(封锁)

  - on demand computing(mining)



### 2.8 BTC脚本

**栈**

+ 交易实例

..................

![image-20201103154024123](.\pic\image-20201103154024123.png)

+ 脚本

```
//以下省略所有OP_
P2PK(pay to public key)
input script:
	PUSHDATA(sig)
output script:
	PUSHDATA(PubKey)
	CHECKSIG

执行：实际应该是分开执行
PUSHDATA(sig)
PUSHDATA(PubKey)
CHECKSIG

P2PKH(pay to public key hash)
input script:
	PUSHDATA(sig)
	PUSHDATA(PubKey)
output script:
	DUP //复制栈顶元素
	HSAH160 //栈顶弹出 hash后压入
	PUSHDATA(PubKeyHash)
	EQUALVERIFY //弹出栈顶两个元素是否相等
	CHECKSIG //弹出栈顶两个元素并检查签名
	
P2SH(pay to Script Hash)
采用BIP16的方案
redeemScript
过程：
第一阶段
PUSHDATA(Sig)
PUSHDATA(seriRS)
HASH160
PUSHDATA(RSH)
EQUAL
第二阶段
PUSHDATA(PubKey)
CHECKSIG

```

+ 多重签名

  ​	CHECKMULTISIG

  ​	用P2SH实现多重签名

```
FALSE
PUSHDATA(Sig1)
PUSHDATA(Sig2)
PUSHDATA(seriRS)
HASH160
PUSHDATA(RSH)
EQUAL
//赎回脚本展开
2
PUSHDATA(pubkey1)
PUSHDATA(pubkey2)
PUSHDATA(pubkey3)
3
CHECKMULTISIG
```

+ Proof of Burn

```
output script
	RETURN
		[zero or more ops](hash值)
//这种形式的output称为：Provably Unspendable/Prunable Output
//应用场景：1小币种生产 2加入内容
//发布交易不需要记账权 发布区块才需要
```



### 2.9 BTC分叉

**fork**

+ state fork 

​		forking attack (deliberate fork)

+ protocol fork(由于修改协议)
  + hard fork
  + soft fork

**hard fork**

​	例子：block size limit (1M --> 7 tx/sec)   假设更新1M --> 4M

​		分裂带来问题 ETH/ETC -->chain ID

​	必须所有节点都要更新才行

**soft fork**

- 临时性的	

- 例子：假设更新1M-->0.5M

- 实际情况：增加新的含义 coinbase （extra nonce）

- 历史例子：P2SH

- 只要有半数以上更新了，就不会出现永久性的分叉



### 问答

---

+ 如果转账时转账接受者没有连入互联网？
不需要知道是否在线，仅仅是记录在链中
+ 有没有可能接受者收款地址从来没有听说过？
可能的
+ 账户私钥丢失怎么办？
没有办法
另外，有些加密货币交易时，一般来说交易所是中心化的机构。在这个情况下可以联系交易所，但当前缺乏监管。
+ 私钥泄露怎么办？
尽快转钱
+ 转账的时候写错了地址？
没有办法取消已发布的交易
+ OP_RETURN实际操作，无条件返回错误如何被验证？
 + 什么叫验证通过？
	验证的时候是把当前输入和前一个输出拼接执行
	RETURN 写在当前输出，不会被执行，即下一个验证的时候才会被使用
+ 如何确定是哪个矿工最先找到nonce？
发布的区块里有一个coinbase tx包含矿工收款地址，修改则会引起hash发生变化
+ 交易费可以看做是小费，但事先如何知道哪一个得到
事先不需要知道哪一个会得到 但total input > total outputs 谁完成就给谁


### 2.10 BTC匿名性
**Bitcoin and anonymity**
pseudonymity 不完全匿名
破坏匿名性：
+ 多个地址账户可能被关联起来（通过交易等）
+ 与现实生活关联（交易所/场外交易/大额交易转入转出/实体世界支付）

hide your identity from whom?
如何尽量提高匿名性：
+ application layer:
coin mixing
+ network layer:
多路径转发（洋葱路由）

**零知识证明**

+ 指一方（证明者）向另一方（验证者）证明一个陈述是正确的，而无需透露除该陈述是正确的外的任何信息。
  同态隐藏

+ 加密函数x,y不等，则E(x),E(y)不等，逆否

+ 给定E(x)很难推出x，不可逆

+ 给定E(x)，E(y)的值，可以很容易计算出来某些关于x,y的加密函数值。
	+ 同态加法
	+ 同态乘法
	+ 拓展到多项式

例：A向B证明她知道x,y使得x+y=7,同时不让B知道具体的值。

简单的版本：
收到E(x),E(y)计算出E(x+y)的值，并计算出E(7)的值，如果E(x+y)==E(7)则验证通过。

**盲签方法**

+ 用户A提供SerialNum，银行在不知道的情况下返回签名Token，减少A的存款
+ 用户A把SerialNum和Token交给B完成
+ 用户B拿给银行验证

+ 银行无法把A和B验证

+ 中心化

**零币和零钞**

+ 零币和零钞在协议层就融合了匿名化处理，其匿名性来自密码学保证
+ zerocoin系统中存在基础币和零币
+ 零钞zerocash系统使用zk-SNARKs协议，不依赖一种基础币



### 思考

1. 哈希指针

形象说法，实际系统用的时候只用hash没用指针

```C++
class CBlockHeader{
public:
    //header
    int32_t nVersion;
    uini256 hashPrevBlock; //哈希指针
    uini256 hashMerkleRoot;
    ...
}
```

存储（key，value） levelDB

2. 区块恋（共享账户）

截断私钥比较明显的问题：当一个人丢失，即丢失；长度减少降低安全性

采用多重签名

3. 分布式共识

为什么比特币系统能够绕过分布式共识中的那些不可能结论？

没有达到真正的共识（分叉攻击）（理论特定模型 和 实际差别）

4. 比特币的稀缺性

能启动的问题

稀缺的东西不适合作为货币

5. 量子计算

不需担心



## 3 以太坊

### 3.1 以太坊概述

区别：

GHOST

memory hard mining puzzle — ASIC resistance （趋势proof of work->proof of stake）

智能合约的支持 smart contract （decentralized contract）

### 3.2 以太坊账户

account-based ledger 基于账户的模型

​	天然防范double spending attack 但replay attack（增加交易次数nonce）

externally owned account 外部账户

​	balance 

​	nonce 计数器

smart contract account 合约账户

​	以上内容 但不能主动发起一个交易

​	code， storage

### 3.3 ETH状态树
账户状态
addr --> state 160bits 40个16进制数
Merkle Tree不适用：账户节点过多、顺序问题不唯一、插入代价

**trie**  (retrieval)
字典树
branch factor 0-F
无碰撞
插入顺序无关
更新局部性

**Patricai tree**
路径压缩前缀树
键值分布比较稀疏

**MTP**
Merkle Patricia tree

+ Merkle tree | binary tree
+ Merkle Patricia tree | Patricai tree

**Modified MPT**
为了支持回滚需要保存历史状态

**数据结构**
...
(key, value) value需要进行RLP(recursive Length Prefix)序列化

### 3.4 ETH交易树和收据树
交易数 <--> 收据树
同样使用**MPT**

**bloom filter**

+ 比较高效查找指定元素是不是在集合里
+ 大的集合计算出紧凑的摘要向量
+ false positive（存在误报，不存在漏报）
+ 01取值的局限性：不支持删除操作
+ 查找哪一个有需要的 再去查找区块里面（去掉许多区块）

**transaction-driven state machine** 交易驱动状态机

Q：为什么状态树要保存所有节点信息
A：查找账户方便，尤其是新建账户



### 3.5 ETH Ghost（共识机制）

出块速度提高-->传播时间较长-->临时性的分叉更频繁-->采用BTC方法很多会被丢弃

**基于GHOST协议的共识机制**

+ 解决临时性分叉

+ GHOST协议

  2个uncle block作废之后有一定奖励 7/8    后一个区块n*1/32

+ 改进
  + 规定前面的都是叔父区块
  + uncle reward出块奖励每上一代减少1/8的奖励 7/8 6/8 5/8.......2/8  at most seven generation 规定隔代长度、鼓励尽早合并 
  + block reward > gas fee
  + 不执行叔父区块
  + 只有分叉后的第一个区块可以得到reward 鼓励及时合并 | 否则减少了分叉攻击的代价



### 3.6 ETH挖矿算法

Block chain is secured by mining.（工作量证明）

mining puzzle 目标：ASIC resistance --> memory hard mining puzzle

设计puzzle 原则：

​	difficult to solve, but easy to verify

**以太坊mining puzzle**

+ 两个数据集16M cache， 1G dataset(DAG，定期增长)

  seed --> cache --> dataset

+ 求puzzle时 根据nonce 从dataset中读取运算 64轮每次2个 得到128数再求

```python
#ETH 伪代码  ethash
#生成cache
def mkcache(cache_size,seed):
	o = [hash(seed)]
	for i in range(1, cache_size)
		o.append(hash(o[-1]))
	return o;
#cache 每隔30000个块重新生成seed 大小增加1/128*16M

#生成大数据集 第i个元素
def calc_dataset_item(cache,i):
    cache_size - cache.size
    mix = hash(cache[i % cache_size] ^ i) #初始
    for j in range(256):
        cache_index = get_int_from_item(mix) #下一个下标
        mix = make_item(mix, cache[cache_index % cache_size])#第i个元素
	return hash(mix)

def calc_dataset(full_size, cache):
     return [calc_dataset_item(cache,i) for i in range(full_size)]
   
#矿工挖矿
def hashimoto_full(header, nonce, full_size, dataset):
    #块头 ， nonce , 数据集中元素个数， 数据集
    mix = hash(header, nonce) #初始
    for i in range(64):  #循环
        dataset_index = get_int_from_item(mix) % full_size
        mix = make_item(mix, dataset[dataset_index])
        mix = make_item(mix, dataset[dataset_index + 1])
    return hash(mix)   #比较结果

#轻节点验证
def hashimoto_light(header, nonce, full_size, cache):
    #块头， 包含在块头里的nonce，大数据集的元素个数 ， cache
    mix = hash(header, nonce)
    for i in range(64):
        dataset_index = get_int_from_item(mix) % full_size
        mix = make_item(mix, calc_dataset_item(cache,dataset_index))
        mix = make_item(mix, calc_dataset_item(cache,dataset_index + 1))
    return hash(mix)     

#挖矿伪代码
def mine(full_size, dataset, header, target):
    nonce = random.randint(0, 2**64)
    while hashimoto_full(header, nonce, full_size,dataset) > target:
        nonce = (nonce + 1) % 2**64
    return nonce
```

+ 以太坊中采用pre-mining



### 3.6 ETH挖矿难度调整

难度调整算法（现行版本，代码为准）
$$
D(H) \equiv 
\begin{cases} 
D_0,&if\ H_i = 0\\
max(D_0,P(H)_{H_d}+x \times \zeta_2) + \epsilon, &otherwise
\end{cases}
\\where D_0\equiv131072
$$
参数说明

+ D(H)是本区块难度，由基础部分$P(H)_{H_d}+x \times \zeta_2$ 和难度炸弹部分$\epsilon$相加得到。 
+ $P(H)_{H_d}$为父区块的难度
+ $x \times \zeta_2$用于自适应调整出块难度
+ 基础部分有下界

$$
x \equiv \lfloor {P(H)_{H_d} \over 2048} \rfloor
$$

$$
\zeta_2 \equiv max(y - \lfloor{{H_s-P(H)_{H_s}} \over 9}\rfloor,-99)
$$

+ x是调整的单位，$\zeta_2$调整系数
+ y和父区块的uncle数有关，如果服区块中包含了uncle，则y=2，否则y=1
  + 包含uncle时新发行的货币量大，需适当提高难度以保持发行量稳定
+ 难度降低上界为-99，主要应对被黑客攻击和想不到的黑天鹅事件
+ H_s是本区块的时间戳，以秒为单位
  + 该部分是稳定出块速度的最重要部分

难度炸弹
$$
\epsilon \equiv \lfloor2^{\lfloor H'_i \div100000 \rfloor-2}\rfloor
\\
H' \equiv max(H_i-3000000,0)
$$

+ 为什么设置难度炸弹

  设置难度炸弹的原因是要降低迁移到PoS协议时发生fork的风险：到时挖矿难度非常大，所以矿工有意愿迁移到PoS协议。

+ 参数说明

  2的指数函数后期增长非常快

  $H'_i$称为fake block number，由真正的减去

以太坊发展的四个阶段

Frontier Homestead Metropolis（Byzantine，Constantinople） Serenity

说明:

+ Metropolis 两个阶段
+ 炸弹回调是Metropolis 子阶段，在EIP中决定

具体代码实现：

```go
func calcDifficultyByzantium(time unit64, parent *types.Header)*big,Int{
    
}
 
```



### 3.7 权益证明

 **Proof of stake**  ------ Proof of work(耗费能源)

virtual mining

**Casper the Finality Gadget(FFG)**

+ Validator 由保证金推动达成共识(epoch two-phase commit prepare/commit message)
+ finality中的交易安全性

为什么一开始不用权益证明

​	——没有得到检验



### 3.8 ETH智能合约

**智能合约**

+ 运行在区块链上的一段代码，代码逻辑定义了合约的内容

+ 智能合约账户保存了合约当前的运行状态

balance nonce code storage

+ Solidity是最常用语言

外部账户如何调用智能合约？

创建一个交易，接受地址为要调用的那个合约的地址，data域填写要调用的函数及其参数的编码值。



一个合约调用另一个合约中的函数？

1、直接调用（实际还需一个外部账户）

2、使用address类型的call()函数

3、代理调用delegatecall()

+ 与call相同只是不能调用.value()

fallback函数

+ 匿名函数

+ 在两种情况下被调用：

直接向一个合约地址转账不加任何data

被调用的函数不存在

+ 如果转账金额不是0，同样需要声明payable

  

智能合约的创建和运行

+ 智能合约的代码写完后，要编译成bytecode
+ 创建合约：外部账户发起一个转账交易到0x0地址
+ 转账金额是0，但是要支付汽油费

- 合约代码放在data域里

+ 智能合约运行在EVM上

+ 以太坊是一个交易驱动的状态机

+ 调用智能合约的交易发布到区块链上后，每个矿工都会执行这个交易，确定性转移到下一个状态

  

**汽油费（gas fee）**

+ 智能合约是一个图灵完备模型

- halting problem 出现死循环

+ 执行合约中的指令要收取汽油费，发起人支付

+ EVM中不同指令消耗的汽油费不同

错误处理（原子性）

+ 智能合约中不存在自定义的try-catch结构

+ 一旦遇到异常，除特殊情况外，本次执行操作全部回滚

+ 可以抛出错误的语句

- assert(bool condition):内部错误

- require：输入或者外部错误

- revert():终止运行并回滚

嵌套调用

+ 合约执行具有原子性

+ 嵌套调用是指一个合约调用另一个合约中的函数

+ 嵌套调用的连锁式回滚？

+ 一个合约直接向一个合约账户转账，没有指明调用哪个函数会引起嵌套调用



**先执行还是先挖矿**？

block header --> 需要先执行合约得到hash，再执行

对于先执行，以太坊中没有补偿，且需要执行验证发布交易



**执行错误的交易是否发布**？

需要发布--扣掉汽油费--交易执行情况



**智能合约是否支持多线程**？

确定性的交易驱动状态机-->多线程多内存访问结果不确定



智能合约可以获得的区块信息

+ blockhash

+ coinbase

+ ....

智能合约可以获得的调用信息

+ ...

智能合约中地址类型：

+ ...

例子：简单拍卖

改版：

记录owner增加超级管理员

问题：需要信任owner 与去中心化矛盾

改版：

由投标者自己取回出价withdraw()函数

问题：重入攻击（re-entrancy attack）

+ 当合约账户收到ETH但未调用函数时，会执行fallback()函数

+ 通过addr.send/transfer/call.value 都会触发

+ fallback()函数由用户自己编写

再改版：

修改调用函数



### 3.9 ETH-The DAO

DAO: decentralized Autonomous Organization

The DAO(区块链上众筹 本质是一个智能合约)

​	工作方式类似于 DAC(... Corporation) 盈利性目的

但仅存活了3个月

投资后取回—splitDAO—也是建立子基金的方法（childDAO）—极端即为1个人

**问题**：

splitDAO代码 （问题：先转账再清零 ）

**补救**：

软件升级（相关区块锁定 软分叉失败 --> 强制退回硬分叉 投票）



### 3.10 The DAO反思
1. 关于智能合约
Is smart contract really smart?
Smart contract is anything but smart.
2. 关于不可篡改性
irrevocability is a double edged sword.
3. 是否真的不可篡改
Nothing is irrevocable.
升级分叉
4. solidity语言设计
？调用
Is solidity the right programming language?
图灵完备是不是一个好事情
5. 开源是否更安全
many eyeball fallacy
实际不会很多人去看，不一定更安全
6. 去中心化
What does decentralization mean?
硬分叉实际上也体现了去中心化->大多数同意
7. decentralized != distributed
去中心化一定是分布式，分布式不一定是去中心化



### 3.11 ETH 美链
beauty chain
+ 一个发行代币的智能合约
+ 函数batchTransfer，给目标转账，执行者账户上扣除
+ 问题：乘法溢出
  解决：完全采用safemath



### 4 总结

应用场景：

+ 问题场景：
  + 保险理赔：时间在于需要人工审核内容
  + 防伪溯源：其实并不知道具体的过程，误记、掉包

商业模式：中心化和去中心化并不是完全区分的。

监管？

发展趋势：

Internet / payment

加密货币并不是要与信用卡竞争\要考虑特定的历史条件

智能合约？

The DAO商业模式？民主是不是好事情？



