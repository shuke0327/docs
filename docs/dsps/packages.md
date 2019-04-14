服务包
========

## 注册
### 准备和托管 dsp.json 
```JSON
{
    "name": "acme DSP",
    "website": "https://acme-dsp.com",
    "code_of_conduct":"https://...",
    "ownership_disclosure" : "https://...",
    "email":"dsp@acme-dsp.com",
    "branding":{
      "logo_256":"https://....",
      "logo_1024":"https://....",
      "logo_svg":"https://...."
    },
    "location": {
      "name": "Atlantis",
      "country": "ATL",
      "latitude": 2.082652,
      "longitude": 1.781132
    },
    "social":{
      "steemit": "",
      "twitter": "",
      "youtube": "",
      "facebook": "",
      "github":"",
      "reddit": "",
      "keybase": "",
      "telegram": "",
      "wechat":""      
    }
    
}

```
### 准备和托管 dsp-package.json 
```JSON
{
    "name": "Package 1",
    "description": "Best for low vgrabs",
    "dsp_json_uri": "https://acme-dsp.com/dsp.json",
    "logo":{
      "logo_256":"https://....",
      "logo_1024":"https://....",
      "logo_svg":"https://...."
    },
    "service_level_agreement": {
        "availability":{
            "uptime_9s": 5
        },
        "performance":{
            "95": 500,
        }
    },
    "pinning":{
        "ttl": 2400,
        "public": false
    },
    "locations":[
        {
          "name": "Atlantis",
          "country": "ATL",
          "latitude": 2.082652,
          "longitude": 1.781132
        }
    ]
}
```
### 如果不使用 Kubernetes
```bash
npm install -g @liquidapps/zeus-cmd
cd $(readlink -f `which setup-dsp` | xargs dirname)
```
### 注册服务包

**警告: 服务包是只读模式，不可移除**

```bash
export PACKAGE_ID=package1
export EOS_CHAIN=mainnet
#或者使用
export EOS_CHAIN=kylin

export DSP_ENDPOINT=https://acme-dsp.com
zeus register dapp-service-provider-package \
    ipfs $DSP_ACCOUNT $PACKAGE_ID \
    --key $DSP_PRIVATE_KEY \
    --min-stake-quantity "10.0000" \
    --package-period 86400 \
    --quota "1.0000" \
    --network $EOS_CHAIN \
    --api-endpoint $DSP_ENDPOINT \
    --package-json-uri https://acme-dsp.com/package1.dsp-package.json
```

输出结果应为:
```
⚡registering package:package1
✔️package:package1 registered successfully
```

更多选项:
```bash
zeus register dapp-service-provider-package --help 
```

#### 需改服务包元数据:
目前只有 `package_json_uri` 及 `api_endpoint` 两者可以修改。

修改元数据，需要使用 dappservices 合约中的 "modifypkg" 操作指令.

使用 cleos:
```bash
cleos -u $EOS_ENDPOINT push action dappservices modifypkg "[\"$DSP_ACCOUNT\",\"$PACKAGE_ID\",\"ipfsservice1\",\"$DSP_ENDPOINT\",\"https://acme-dsp.com/modified-package1.dsp-package.json\"]" -p $DSP_ACCOUNT@active
```