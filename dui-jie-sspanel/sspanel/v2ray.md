# 对接V2ray

| 协议 | 支持情况 |
| :--- | :--- |
| VMess | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls |
| VMessAEAD | tcp, tcp+tls, ws, ws+tls, h2c, h2+tls |
| VLess | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |

## SSpanel-uim 节点地址格式

```text
IP;监听端口;alterId;(tcp或ws);(tls或不填);path=/xxx|host=xxxx.com|server=xxx.com|outside_port=xxx
```

alterId设为0，则自动启用VMessAEAD。

## TCP示例

```text
ip;12345;2;tcp;;server=域名
```

```text
示例：1.3.5.7;12345;2;tcp;;server=hk.domain.com
```

## tcp + tls 示例

```text
ip;12345;2;tcp;tls;server=域名|host=域名
```

```text
示例：1.3.5.7;12345;2;tcp;tls;server=hk.domain.com|host=hk.domain.com
```

## ws示例

```text
ip;80;2;ws;;path=/xxx|server=域名|host=CDN域名
```

```text
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## ws + tls 示例

```text
ip;443;2;tls;ws;path=/xxx|server=域名|host=CDN域名
```

```text
示例：1.3.5.7;443;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## ws + tls \(Caddy/Nginx\) 示例

交由Caddy或者Nginx处理TLS 节点配置和 ws+tls一致，在后端配置`CertMode: none`

同时设置outside\_port为Caddy/Nginx监听端口，转发到12345为XrayR监听端口。可以在后端配置`ListenIP: 127.0.0.1`监听本地端口。

```text
ip;12345;2;tls;ws;path=/xxx|server=域名|host=CDN域名|outside_port=443
```

```text
示例：1.3.5.7;12345;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com示例：1.3.5.7;12345;2;ws;tls;path=/v2ray|server=hk.domain.com|host=hk.domain.com
```

## 中转端口

在任一配置组合后增加`|outside_port=xxx`,此项为用户连接端口。

XrayR没有`inside_port=xx`配置选项，如需监听本地端口，请在配置文件中设置监听ip为`127.0.0.1`。

```text
示例：1.3.5.7;80;2;ws;;path=/v2ray|server=hk.domain.com|host=hk.domain.com|outside_port=12345
```

## 启用Vless **\(此项为实验性功能\)**

此项为实验性功能，请确保您使用的面板已经支持下发vless订阅，否则请手动配置客户端。

在本地设置文件将`EnableVless`设为true。 配置文件详见：[配置文件说明](../../xrayr-pei-zhi-wen-jian-shuo-ming/config.md#mian-ban-dui-jie-pei-zhi)

请开启vless同时务必使用tls或者xtls。

## 启用xtls **\(此项为实验性功能\)**

此项为实验性功能，请确保您使用的面板已经支持下发带有xtls的订阅，否则请手动配置客户端。

在本地设置文件将`EnableXTLS`设为true。 配置文件详见：[配置文件说明](../../xrayr-pei-zhi-wen-jian-shuo-ming/config.md#mian-ban-dui-jie-pei-zhi)

