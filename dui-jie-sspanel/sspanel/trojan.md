# 对接Trojan

| 协议 | 支持情况 |
| :--- | :--- |
| Trojan | √ |

## SSpanel-uim 节点地址格式

```text
域名或IP;port=用户连接端口#监听端口|host=xx
```

## 直连示例

```text
示例：gz.aaa.com;port=443|host=gz.aaa.com
```

## 中转示例

用户连接443，XrayR监听12345

```text
示例：gz.aaa.com;port=443#12345|host=hk.aaa.com
```

## 启用xtls **\(此项为实验性功能\)**

在本地设置文件将`EnableXTLS`设为true。 配置文件详见：[配置文件说明](https://github.com/XrayR-project/XrayR-doc/tree/af55d4cc45735ca8d00491aa97f8cbbd97c8faf4/sspanel/config/README.md)

