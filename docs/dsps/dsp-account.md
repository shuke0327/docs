账号
=======

## 准备条件
安装cleos: https://github.com/EOSIO/eos/releases

## 账户名

```bash
# 设置可用的账户名 (将下文中的 'yourdspaccount' 替换为你的账号名):
export DSP_ACCOUNT=yourdspaccount

# 创建钱包 wallet
cleos wallet create --file wallet_password.pwd
```
*将 wallet_password.pwd 文件保存在安全的位置!*

## 创建账号
### 主网账号创建

```bash
cleos create key --to-console > keys.txt
export DSP_PRIVATE_KEY=`cat keys.txt | head -n 1 | cut -d ":" -f 2 | xargs echo`
export DSP_PUBLIC_KEY=`cat keys.txt | tail -n 1 | cut -d ":" -f 2 | xargs echo`
```
*将 keys.txt 保存在安全的位置!*

#### 已有账号
- [EOS 主网上手指南](https://hackernoon.com/getting-started-on-eos-mainnet-in-10-minutes-bf61dd9ec787)

#### 创建第一个 EOS 账号
使用法币:
- [EOS Account Creator](https://eos-account-creator.com/)
- [EOS Lynx](https://eoslynx.com/)

使用Bitcoin/ETH/Bitcoin Cash/ALFAcoins:
- [ZEOS](https://www.zeos.co/)

### Kylin
创建账号
```bash
curl http://faucet.cryptokylin.io/create_account?$DSP_ACCOUNT > keys.json
curl http://faucet.cryptokylin.io/get_token?$DSP_ACCOUNT
export DSP_PRIVATE_KEY=`cat keys.json | jq -e '.keys.active_key.private'`
export DSP_PUBLIC_KEY=`cat keys.json | jq -e '.keys.active_key.public'`
```
*将 keys.json 保存在安全位置!*

## 导入账号
```bash
cleos wallet import $DSP_PRIVATE_KEY
```
