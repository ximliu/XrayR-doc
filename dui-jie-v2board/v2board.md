# 基本对接配置

1. 在`config.yml`中配置`PanelType: "V2board"`。
2. V2board只有V2ray节点类型支持审计规则。
3. 启用vless和xtls，请在配置文件中手动启动，V2board不支持在线配置。

配置文件详见：[配置文件说明](../xrayr-pei-zhi-wen-jian-shuo-ming/config.md)

### 对接vmess+grpc

为了成功支持clash连接，在对接vmess+grpc时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "serviceName": "host",
}
```

，其中`"name"`换成你的域名，或其他sni分流域名

### 对接vmess+tcp+http

在对接vmess+tcp+http时，v2board需要在传输协议配置中增加如下内容：

```text
{
  "header": {
    "type": "http",
    "request": {},
    "response": {}
  }
}
```

其中`request`和`response`中的内容请自行参照[Xray-core文档](https://xtls.github.io/config/transports/tcp.html#httpheaderobject)设置。

