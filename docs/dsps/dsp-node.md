DSP 节点
========
## 硬件要求

## 预装要求
### Linux
```bash
sudo su -
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
export NVM_DIR="${XDG_CONFIG_HOME/:-$HOME/.}nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
nvm install 10
nvm use 10
exit
```

#### Ubuntu/Debian
```bash
sudo apt install -y make cmake build-essential python
```

#### Centos/Fedora/AWS Linux:
```bash
sudo yum install -y make cmake3 python
```

## 安装
```bash
sudo su -
nvm use 10
npm install -g pm2
npm install -g @liquidapps/dsp --unsafe-perm=true
exit
```
## 配置
```bash
sudo su -
setup-dsp
systemctl stop dsp
systemctl start dsp
systemctl enable dsp
exit
```

自动填写如下细节信息:
### Demux 后端
DEMUX_BACKEND 

- state_history_plugin 
- zmq_plugin - only if using nodeos with eosrio's version of the ZMQ plugin: https://github.com/eosrio/eos_zmq_plugin

### IPFS 集群
IPFS_HOST - ipfs 主机地址
IPFS_PORT (5001) - ipfs 端口
IPFS_PROTOCOL (http) - ipfs 协议

[IPFS 集群](ipfs-cluster)的主机地址，端口及协议

### DSP 账号

DSP_ACCOUNT and DSP_PRIVATE_KEY - [所创建 DSP 账号](dsp-account)的账户和私钥

### nodeos ENVS
[EOS Node 设置](eosio-node)

NODEOS_HOST - nodeos 主机地址

NODEOS_PORT (8888) - nodeos 端口 

NODEOS_ZMQ_PORT (5557) - 如果使用 zmq_plugin

NODEOS_WEBSOCKET_PORT (8887) - 如果使用 state_history_plugin

NODEOS_CHAINID:

 - 主网 chainID: aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906
 - 麒麟测试网 chainID: 5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191


## 查看 logs
```bash
sudo pm2 logs
```

输入结果类似:
```
1|demux    | demux listening on port 3195!
1|demux    | ws connected
1|demux    | got abi
0|dapp-services-node  | services listening on port 3115!
0|dapp-services-node  | service node webhook listening on port 8812!
2|ipfs-dapp-service-node  | ipfs listening on port 13115!
2|ipfs-dapp-service-node  | commited to: ipfs://zb2rhmy65F3REf8SZp7De11gxtECBGgUKaLdiDj7MCGCHxbDW
2|ipfs-dapp-service-node  | ipfs connection established
```
