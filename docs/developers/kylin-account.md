Kylin 测试账号
=====================
## 账号

```bash
# 设置可用的账户名 (将下文中的 'yourtestaccount' 替换为你的账号名):
export KYLIN_TEST_ACCOUNT=yourtestaccount
# 创建钱包 wallet
cleos wallet create --file wallet_password.pwd

# 创建账号 Account
curl http://faucet.cryptokylin.io/create_account?$KYLIN_TEST_ACCOUNT > keys.json
export KYLIN_TEST_PRIVATE_KEY=`cat keys.json | jq -e '.keys.active_key.private'`
export KYLIN_TEST_PUBLIC_KEY=`cat keys.json | jq -e '.keys.active_key.public'`
cleos wallet import $KYLIN_TEST_PRIVATE_KEY

# 配置 endpoint
export EOS_ENDPOINT=https://kylin-dsp-1.liquidapps.io
```
*请务必将 wallet_password.pwd 和 keys.json 安全保存!*

## Kylin EOS 代币
```bash
# 获取 Kylin EOS 代币
curl http://faucet.cryptokylin.io/get_token?$KYLIN_TEST_ACCOUNT
```

## Kylin DAPP 代币

[DAPP 水龙头](https://kylin-dapp-faucet.liquidapps.io/)
