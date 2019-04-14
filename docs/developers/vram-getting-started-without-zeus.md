vRAM 入门 - 不适用 zeus
===================================

```            
       _____            __  __ 
      |  __ \     /\   |  \/  |
__   _| |__) |   /  \  | \  / |
\ \ / /  _  /   / /\ \ | |\/| |
 \ V /| | \ \  / ____ \| |  | |
  \_/ |_|  \_\/_/    \_\_|  |_|
            
```

## 硬件要求

## 预备条件

* [eosio.cdt v1.6.1](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.1)
* [eosio v1.7.1](https://github.com/EOSIO/eos/releases/tag/v1.7.1)
* [Kylin 测试网络账号](kylin-account.md)

## 安装

复制到 `/contracts` 文件夹:
```bash
git clone --recursive https://github.com/liquidapps-io/****UPDATE_THIS****
```


## 修改合约

vRAM提供了对multi_index表的替换，而multi_index表也与传统的multi_index表以相同的方式进行交互，因此使用起来非常简单和熟悉。

想要使用 vRAM 数据表，将如下的代码加入到智能合约之中:


```cpp
#include "../dappservices/log.hpp"
#include "../dappservices/multi_index.hpp"

#define DAPPSERVICES_ACTIONS()
    XSIGNAL_DAPPSERVICE_ACTION
    LOG_DAPPSERVICE_ACTIONS
    IPFS_DAPPSERVICE_ACTIONS

#define DAPPSERVICE_ACTIONS_COMMANDS()
    IPFS_SVC_COMMANDS()LOG_SVC_COMMANDS()

CONTRACT_START()

...

/*** REPLACE eosio : ***/
typedef eosio::multi_index<"accounts"_n, account> accounts_t;

/*** WITH    dapp  : ***/
typedef dapp::multi_index<"accounts"_n, account> accounts_t;

...

CONTRACT_END((youraction1)(youraction2)(youraction2))
```

## 编译
```bash
eosio-cpp -abigen -o contract.wasm contract.cpp
```

## 部署合约
```bash
# 购买 RAM:
cleos -u $EOS_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "50.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $EOS_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active

# 设定合约权限
cleos -u $EOS_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[\"$KYLIN_TEST_PUBLIC_KEY\"],\"accounts\":[{\"permission\":{\"actor\":\"eosio.code\",\"permission\":\"active\"},\"weight\":1}]}" active -p $KYLIN_TEST_ACCOUNT@active
```

## 选择 DSP 服务包，抵押 DAPP 代币

[DSP服务包及代币抵押](dsp-packages-and-staking.md)

## 测试

最后，您现在可以通过 DSP 的 API 端口发送操作指令(action)来测试 vRAM 的实现


API 可以在 [服务包的数据表中找到](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100)。在所有可用的区块链上，都放在账号 `dappservices`下。

```bash
export EOS_ENDPOINT=https://dspendpoint
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active
```

结果类似:
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```