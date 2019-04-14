Zeus 入门
====================

## 概述
zeus-cmd 是一个可扩展的命令行工具. SDK 的扩展称之为 "boxes".

## Boxes

* EOSIO dApp 开发支持
* DAPP 服务支持

## 硬件要求

## 预备环境

* nodejs == 10.x (nvm recommended)
* curl

推荐 (否则会回退至 docker)
* [eosio.cdt v1.6.1](https://github.com/EOSIO/eosio.cdt/releases/tag/v1.6.1)
* [eosio v1.7.1](https://github.com/EOSIO/eos/releases/tag/v1.7.1)


## 安装 Zeus

```bash
npm install -g @liquidapps/zeus-cmd
```

### 在 Mac 上 docker的版本推荐
推荐使用 docker 版本: 18.06.1-ce-mac73

## 升级

```bash
npm update -g @liquidapps/zeus-cmd
```

## 测试
```bash
zeus unbox helloworld
cd helloworld
zeus test
```

## 示例 Boxes
### vRAM
* [coldtoken](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/coldtoken) - vRAM based eosio.token
* [deepfreeze](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/deepfreeze) - vRAM based cold storage contract
* [vgrab](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/vgrab) - vRAM based airgrab for eosio.token
* [cardgame](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/sample/cardgame) - vRAM supported elemental battles
* [registry](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/registry) - Generic Registry - the1registry

### Zeus 扩展
* [contract-migrations-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/contract-migrations-extensions)
* [build-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/build-extensions)
* [test-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/core/test-extensions)
* [eos-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/eos-extensions)
* [unbox-extensions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/repos/unbox-extensions)
* [demux](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/microservices/demux)

### DAPP 服务
* [ipfs-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/ipfs-dapp-service)
* [log-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/log-dapp-service)
* [cron-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/cron-dapp-service)
* [oracle-dapp-service](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/services/oracle-dapp-service)

### Misc.
* [microauctions](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/microauctions) - Micro Auctions
* [eos-detective-reports](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/economics/eos-detective-reports) - EOS Detective Reports - by EOSNation
* [helloworld](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-sdk/sample-eos-cpp) - Hello World
* [token](https://github.com/liquidapps-io/zeus-sdk/tree/master/boxes/groups/eos-framework/token) - Standard eosio.token

## 其他选项
```bash
zeus compile #compile contracts
zeus migrate #migrate contracts (deploy to local eos.node)
```

## 在项目中使用
```bash
zeus --help 
```

### 列出 Boxes
```bash
zeus list-boxes
```

## 项目结构
### 文件夹结构
```
    extensions/
    contracts/
    frontends/
    models/
    test/
    migrations/
    utils/
    services/
    zeus-box.json
    zeus-config.js
```
### zeus-box.json
```
    {
      "ignore": [
        "README.md"
      ],
      "commands": {
        "Compile contracts": "zeus compile",
        "Migrate contracts": "zeus migrate",
        "Test contracts": "zeus test"
      },
      "install":{
          "npm": {
              
          }
      },
      "hooks": {
        "post-unpack": "echo hello"
      }
    }
```

### zeus-config.js
```
    module.exports = {
        defaultArgs:{
          chain:"eos",
          network:"development"
        },
        chains:{
            eos:{
                networks: {
                    development: {
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    },
                    jungle: {
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    },
                    mainnet:{
                        host: "localhost",
                        port: 7545,
                        network_id: "*", // Match any network id
                        secured: false
                    }
                }
            }
        }
    };
```

## 有关权限错误的说明:
推荐使用 NVM
```bash
sudo apt install curl
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
exec bash
nvm install 10
nvm use 10
```
或者尝试如下:
```bash
sudo groupadd docker
sudo usermod -aG docker $USER

#如果仍然遇到错误:
sudo chmod 666 /var/run/docker.sock
```