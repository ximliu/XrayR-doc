# 对接Trojan

| 协议   | 支持情况 |
| ------ | -------- |
| Trojan | √        |

## SSpanel-uim 节点地址格式
```
域名或IP;port=监听端口#用户连接端口|host=xx
```
## 直连示例
```
示例：gz.aaa.com;port=443|host=gz.aaa.com
```
## 中转示例
```
示例：gz.aaa.com;port=443#12345|host=hk.aaa.com
```
## 启用xtls **(此项为实验性功能)**
在配置后增加`|enable_xtls=true`.
```
示例：gz.aaa.com;port=443#12345|host=hk.aaa.com|enable_xtls=true
```

