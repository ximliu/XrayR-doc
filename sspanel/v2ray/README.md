# 对接V2ray

| 协议      | 支持情况                                             |
| --------- | ---------------------------------------------------- |
| VMess     | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls                |
| VMessAEAD | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls                |
| VLess     | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |

## SSpanel-uim 节点地址格式
```
IP;监听端口;alterId;(tcp或ws);(tls或不填);path=/xxx|host=xxxx.com|server=xxx.com|outside_port=xxx
```
alterId设为0，则自动启用VMessAEAD。
## TCP示例
```
ip;12345;2;tcp;;server=域名
```
```
示例：1.3.5.7;12345;2;tcp;;server=hk.domain.com
```
## tcp + tls 示例
```
ip;12345;2;tcp;tls;server=域名|host=域名
```
```
示例：1.3.5.7;12345;2;tcp;tls;server=hk.domain.com|host=hk.domain.com
```
## ws示例
```
ip;80;2;ws;;path=/xxx|server=域名|host=CDN域名
```
```
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```
## ws + tls 示例
```
ip;443;2;tls;ws;path=/xxx|server=域名|host=CDN域名
```
```
示例：1.3.5.7;443;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```
## 中转端口
在任一配置组合后增加`|outside_port=xxx`,此项为用户连接端口。

XrayR没有`inside_port=xx`配置选项，如需监听本地端口，请在配置文件中设置监听ip为`127.0.0.1`。
```
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com|outside_port=12345
```
## 启用Vless **(此项为实验性功能)**

在本地设置文件将`EnableVless`设为true。
配置文件详见：[配置文件说明](../config/README.md)

请开启vless同时务必使用tls或者xtls。
## 启用xtls **(此项为实验性功能)**
在本地设置文件将`EnableXTLS`设为true。
配置文件详见：[配置文件说明](../config/README.md)