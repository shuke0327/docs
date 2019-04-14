测试
=======

## 测试 DSP

###  使用 DSP 运行示例合约:
如果在远程服务器运行, 请参照 [vRAM 上手指南](../developers/vram-getting-started.md)

### 查看 DSP 节点的 log
```bash
pm2 logs
```

使用 kubernetes:
```bash
kubectl logs dsp-dspnode-0 -c dspnode-ipfs-svc
```

### 查找合约所用的 "xcommit" and "xcleanup" 操作指令(actions):

[mycoldtoken1 at bloks.io](https://bloks.io/account/mycoldtoken1)

