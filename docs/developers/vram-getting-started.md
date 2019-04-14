vRAM 入门
====================

```            
       _____            __  __ 
      |  __ \     /\   |  \/  |
__   _| |__) |   /  \  | \  / |
\ \ / /  _  /   / /\ \ | |\/| |
 \ V /| | \ \  / ____ \| |  | |
  \_/ |_|  \_\/_/    \_\_|  |_|
            
```
## 先决条件

* [Zeus](zeus-getting-started.md)
* [Kylin 账号](kylin-account.md)

## Unbox 模板
```bash
zeus unbox vram-getting-started
```
## 添加合约逻辑
in contract/eos/mycontract/mycontract.cpp
```cpp
/* TODO */
```

## 添加合约测试
in tests/mycontract.spec.js
```javascript
// TODO
```
## 编译和测试
```bash
zeus test
```

## 部署合约
```bash
export EOS_ENDPOINT=https://kylin-dsp-1.liquidapps.io
# 购买 RAM:
cleos -u $EOS_ENDPOINT system buyram $KYLIN_TEST_ACCOUNT $KYLIN_TEST_ACCOUNT "50.0000 EOS" -p $KYLIN_TEST_ACCOUNT@active
# Set contract code and abi
cleos -u $EOS_ENDPOINT set contract $KYLIN_TEST_ACCOUNT ../contract -p $KYLIN_TEST_ACCOUNT@active

# 设置合约权限
cleos -u $EOS_ENDPOINT set account permission $KYLIN_TEST_ACCOUNT active "{\"threshold\":1,\"keys\":[{\"weight\":1,\"key\":\"$KYLIN_TEST_PUBLIC_KEY\"}],\"accounts\":[{\"permission\":{\"actor\":\"$KYLIN_TEST_ACCOUNT\",\"permission\":\"eosio.code\"},\"weight\":1}]}" owner -p $KYLIN_TEST_ACCOUNT@active
```


```bash
# TBD: 
#   zeus import key $KYLIN_TEST_ACCOUNT $KYLIN_TEST_PRIVATE_KEY
#   zeus create contract-deployment contractcode $KYLIN_TEST_ACCOUNT
#   zeus migrate --network=kylin
```

## 选择 DSP 服务包并抵押 DAPP 代币

```bash
export PROVIDER=uuddlrlrbass
export PACKAGE_ID=package1
export MY_ACCOUNT=$KYLIN_TEST_ACCOUNT

# 选择服务包: 
export SERVICE=ipfsservice1
cleos -u $EOS_ENDPOINT push action dappservices selectpkg "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $MY_ACCOUNT@active

# 抵押 DAPP 代币，选择合适的服务包:
cleos -u $EOS_ENDPOINT push action dappservices stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]" -p $MY_ACCOUNT@active
```

[服务包及抵押](dsp-packages-and-staking.md)

## 测试

最后，您现在可以通过 DSP 的 API 端口发送操作指令(action)来测试 vRAM 的实现

```bash
cleos -u $EOS_ENDPOINT push action $KYLIN_TEST_ACCOUNT youraction1 "[\"param1\",\"param2\"]" -p $KYLIN_TEST_ACCOUNT@active
```

结果类似
```
executed transaction: 865a3779b3623eab94aa2e2672b36dfec9627c2983c379717f5225e43ac2b74a  104 bytes  67049 us
#  yourcontract <= yourcontract::youraction1         {"param1":"param1","param2":"param2"}
>> {"version":"1.0","etype":"service_request","payer":"yourcontract","service":"ipfsservice1","action":"commit","provider":"","data":"DH......"}
```