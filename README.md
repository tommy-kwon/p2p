# P2P 借贷系统设计

#角色： 
* 管理员:
* 银行：
* P2P借贷公司
* 个人 或 公司

##管理员的接口

* 1. approveBank(address, name) 认证银行
* 2. approveP2P(address, name)  认证P2P借贷公司

##银行的接口

* sendCNY(address, amount) 给用户发比特人民币(BitCNY or BitUSD)

关于 比特人民币的一个注解：

在证券市场市场上，我们都熟悉银证转账，
银行转入证券的资金，不能用于用户之间的转账，
只能用于购买证券。我们也限制比特人民币的功能，
比特人民币只能用在户在P2P市场上购买债券。

##P2P借贷公司的接口

* approveAccount(address, hash)  对用户做实名认证, hash of (name + id + note)
* approveCoin(id) 对债券做审核

## 借方（有闲钱的人）

* register(name, id, note)  把资料提交给P2P公司注册资料
* check(name, id, note)     检查某个用户资料是否正确
* getCNY(amount)            请求进行银证转账，银行会调用sendCNY 发送给用户币. 
* purchase(bytes32 name, uint value) 认购某个债券

## 贷方（要借钱的人）

* register(name, id, note)     把资料提交给P2P公司 
* newCoin(...)                 发行债券
* sendCNY(bankaddress, amount) 贷方拿着比特人民币去银行换成资金，用于生产活动。
* redeem(name, user)           债券到期，赎回对方的债券。


## 交易债券之间的转让

* placeOrder(offerCurrency, offerValue, wantCurrency, wantValue) 放入订单
* matchOrder(orderId)  配对
* cancelOrder(orderId) 撤单
* orders()             订单列表

##All  Pages：

####帐户

在线开户：
* 选择以太坊账户，开户，提交用户资料。

校验身份:
* 检查投资者资料。

个人帐户中心:
* 个人帐户的信息（人民币的余额）
* 个人发行的债券
* 个人购买的债券

####银行

银行管理后台：
* 管理后台检查到银证转账申请，sendCNY,发送比特人民币给对方。

####P2P公司

P2P贷款公司后台：

* 审核用户资料
* 审核债券发行
* 审核资产抵押

####债券

债券列表
添加债券

####二级市场交易

卖出：
* 卖出债券
* 可购买列表

买入：
* 买入债券
* 可卖出债券列表

#背景说明

###P2P 小额贷款 区块链实现：

现在P2P小额贷款公司存在的问题：

1. 经手资金，内部猫腻多。存在圈钱跑路的风险。
2. 数据不公开透明，存在篡改风险。
一旦跑路还可能销毁数据库，使得证据缺失。
3. 流动性差，购买的债券只等等待到期兑现，没有一个市场进行转让

###区块链版本可以很好的解决上面的三个问题：

1. P2P公司不经手用户资金，用户资金完全和银行对接，和P2P公司无关。而且，用户资金冲入银行后，换成的比特人民币可以投资多家P2P公司。
2. 数据完全公开透明，P2P公司主要负责牵线搭桥，审核用户资料，无法凭空产生数据，也无法修改数据。
3. 有一个统一的债券转让市场，急用钱的时候可以在这里抛售转让。

#流程说明：


