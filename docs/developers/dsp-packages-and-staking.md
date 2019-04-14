服务包及代币抵押
====================

## 可用服务包列表

在所支持的区块链上的 dappservices 账号下，DSP 所注册的服务包可以在其中的[服务包数据表中找到](https://kylin.eosx.io/account/dappservices?mode=contract&sub=tables&table=package&lowerBound=&upperBound=&limit=100)。

## 选择 DSP 服务包

选择符合你意愿的 DSP 服务包。

```bash
export PROVIDER=someprovider
export PACKAGE_ID=providerpackage
export MY_ACCOUNT=myaccount

# 选择服务包: 
export SERVICE=ipfsservice1
cleos -u $EOS_ENDPOINT push action dappservices selectpkg "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"$PACKAGE_ID\"]" -p $MY_ACCOUNT@active
```

##  为 DSP 服务包抵押 DAPP 代币
```bash
# 抵押 DAPP 代币: 
cleos -u $EOS_ENDPOINT push action dappservices stake "[\"$MY_ACCOUNT\",\"$PROVIDER\",\"$SERVICE\",\"50.0000 DAPP\"]' -p $MY_ACCOUNT@active
```