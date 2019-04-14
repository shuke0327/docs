# 使用 K8S 在主网部署 DAPP-DSP
This helm chart will install an full mainnet DSP cluster (Syncd Mainnet API Node, DAPP DSP Services, IPFS Cluster)
这一 helm chart 会安装一个完整的主网 DSP 集群(同步主网 API 节点，DAPP DSP 服务，IPFS 集群)

## 集群的最低要求

* CPU: 4x2.2GHz Cores
* 内存: 64GB 内存
* 网络: 1 GigE
* 硬盘: 1TB

## 上手指南
### AWS
[EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)

### GCP 
[GKE](https://cloud.google.com/kubernetes-engine/docs/quickstart)

## 部署
###  安装和配置好 kubectl，并运行 nodes
```
kubectl get nodes
```
### 运行 boostrap 容器 
供 GCP 使用:
```bash
docker run -v /google/google-cloud-sdk:/google/google-cloud-sdk \
    --entrypoint /bin/bash --rm -it -v $HOME/.kube/config:/root/.kube/config \
    liquidapps/zeus-dsp-bootstrap 
```

其他:
```bash
docker run --entrypoint /bin/bash --rm -it -v $HOME/.kube/config:/root/.kube/config \
    liquidapps/zeus-dsp-bootstrap 
```

在容器的 shell 内:
```bash

# 创建 tiller 服务账号
kubectl -n kube-system create serviceaccount tiller
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

# 在集群上安装 helm
helm init --service-account tiller
helm repo update
```

### 编辑 values.yaml (可选)
### 部署
从快照恢复:
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey
```
或使用全备份（full backup）文件进行恢复并重放(replay):
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey --full-replay=true 
```
或先还原后恢复:
```bash
zeus deploy dapp-cluster dspaccount --key yourdspprivatekey --snapshot=false
```

### 监控恢复和同步进度
```bash
kubectl logs -f dsp-nodeos-0 --all-containers
```
*取决于网络连接和硬件性能不同，使用区块链备份进行恢复可能需要几个小时*


### 获得 API 端点(API endpoint) 
AWS:
```bash
DSP_ENDPOINT=$(kubectl get service dsp-dspnode -o jsonpath="{.status.loadBalancer.ingress[?(@.hostname)].hostname}"):3115
echo $DSP_ENDPOINT
```

GCP:
```bash
DSP_ENDPOINT=$(kubectl get service dsp-dspnode -o jsonpath="{.status.loadBalancer.ingress[0].ip}"):3115
echo $DSP_ENDPOINT
```