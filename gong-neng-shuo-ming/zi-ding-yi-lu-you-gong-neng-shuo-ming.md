# 自定义路由功能说明

XrayR完整支持全部的Xray-core所提供的自定义路由功能，具体启用方式如下：

1. 编写 route.json文件，此配置与Xray 路由配置完全相同，请查看：[https://xtls.github.io/config/base/routing/](https://xtls.github.io/config/base/routing/)获取帮助。
2. 在`config.yml`中配置`RouteConfigPath`为route.json的路径。
3. 如果要启用geoip相关配置，请确保`geoip.dat`和`geosite.dat`处于和`config.yml`同一目录。

### 自定义路由功能示例

```text
{
    "domainStrategy": "IPOnDemand",
    "rules": [
        {
            "type": "field",
            "outboundTag": "block",
            "ip": [
                "geoip:private"
            ]
        },
        {
            "type": "field",
            "outboundTag": "block",
            "protocol": [
                "bittorrent"
            ]
        },
        {
            "type": "field",
            "outboundTag": "IPv6_out",
            "domain": [
                "geosite:netflix"
            ]
        }
    ]
}
```

