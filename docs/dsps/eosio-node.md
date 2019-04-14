EOSIO 节点
==========
## 硬件需求
## 预装要求

- jq
- wget
- curl

## 获取 EOSIO 二进制文件

```bash
# install nodeos
VERSION=1.7.1
```

### Ubuntu 18.04
```bash
FILENAME=eosio_$VERSION-1-ubuntu-18.04_amd64.deb
INSTALL_TOOL=apt
```

### Ubuntu 16.04
```bash
FILENAME=eosio_$VERSION-1-ubuntu-16.04_amd64.deb
INSTALL_TOOL=apt
```

### Fedora
```bash
FILENAME=eosio_$VERSION-1.fc27.x86_64.rpm
INSTALL_TOOL=yum
```

### Centos
```bash
FILENAME=eosio_$VERSION-1.el7.x86_64.rpm
INSTALL_TOOL=yum
```

## 安装
```bash
wget https://github.com/EOSIO/eos/releases/download/v$VERSION/$FILENAME
sudo $INSTALL_TOOL install ./$FILENAME
```


## 准备文件夹
```bash
#清理
rm -rf $HOME/.local/share/eosio/nodeos || true

#创建文件夹
mkdir $HOME/.local/share/eosio/nodeos/data/blocks -p
mkdir $HOME/.local/share/eosio/nodeos/data/snapshots -p
mkdir $HOME/.local/share/eosio/nodeos/config -p
```

### Kylin
```bash
URL=https://s3-ap-northeast-1.amazonaws.com/eosbeijing/snapshot-0276f607955f3008bae69fc47a23ac2eb989af1adebeced2d7462ef30423b194.bin
P2P_FILE=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/fullnode/config/config.ini
GENESIS=https://raw.githubusercontent.com/cryptokylin/CryptoKylin-Testnet/master/genesis.json
CHAIN_STATE_SIZE=65535
wget $URL -O $HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin
```        

### 主网

```bash
URL=$(wget --quiet "https://eosnode.tools/api/bundle" -O- | jq -r '.data.snapshot.s3')
P2P_FILE=https://eosnodes.privex.io/?config=1
GENESIS=https://raw.githubusercontent.com/CryptoLions/EOS-MainNet/master/genesis.json
CHAIN_STATE_SIZE=131072
cd $HOME/.local/share/eosio/nodeos/data
wget $URL -O - | tar xvz
SNAPFILE=`ls snapshots/*.bin | head -n 1 | xargs -n 1 basename`
mv snapshots/$SNAPFILE snapshots/boot.bin
```        

## 配置

```bash
cd $HOME/.local/share/eosio/nodeos/config

# 下载 genesis
wget $GENESIS
# 配置
cat <<EOF >> $HOME/.local/share/eosio/nodeos/config/config.ini
agent-name = "DSP"
p2p-server-address = addr:8888
http-server-address = 0.0.0.0:8888
p2p-listen-endpoint = 0.0.0.0:9876
blocks-dir = "blocks"
abi-serializer-max-time-ms = 3000
max-transaction-time = 150000
wasm-runtime = wabt
reversible-blocks-db-size-mb = 1024
contracts-console = true
p2p-max-nodes-per-host = 1
allowed-connection = any
max-clients = 100
network-version-match = 1 
sync-fetch-span = 500
connection-cleanup-period = 30
http-validate-host = false
access-control-allow-origin = *
access-control-allow-headers = *
access-control-allow-credentials = false
verbose-http-errors = true
http-threads=8
net-threads=8
plugin = eosio::producer_plugin
plugin = eosio::chain_plugin
plugin = eosio::chain_api_plugin
plugin = eosio::net_plugin
plugin = eosio::state_history_plugin
trace-history = true
state-history-endpoint = 0.0.0.0:8887
chain-state-db-size-mb = $CHAIN_STATE_SIZE
EOF

curl $P2P_FILE > p2p-config.ini
cat p2p-config.ini | grep "p2p-peer-address" >> $HOME/.local/share/eosio/nodeos/config/config.ini
```

## 运行 
首次运行(从快照文件读取)
```bash
nodeos --disable-replay-opts --snapshot $HOME/.local/share/eosio/nodeos/data/snapshots/boot.bin --delete-all-blocks
```

等待节点完全同步后，按下 CTRL+C 使其断开一下，等待 node 停止，然后尽心下一步。

## systemd
```bash
export NODEOS_EXEC=`which nodeos`
export NODEOS_USER=$USER
sudo -E su - -p
cat <<EOF > /lib/systemd/system/nodeos.service
[Unit]
Description=nodeos
After=network.target
[Service]
User=$NODEOS_USER
ExecStart=$NODEOS_EXEC --disable-replay-opts
[Install]
WantedBy=multiuser.target
EOF

systemctl start nodeos
systemctl enable nodeos
exit
sleep 3
systemctl status nodeos
```

## 优化

- [atticlab - cpu performance presentation](https://github.com/atticlab/eos-bp-performance/blob/master/cpu_perf_presentation.pdf)
