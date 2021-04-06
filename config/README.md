# 配置文件说明

## 配置文件格式
1. 主配置文件采用`yaml`格式，命名为`xxx.yml`。
2. 默认XrayR会使用软件运行目录下的`config.yml`作为配置文件。

配置文件基本格式，Nodes下可以同时添加多个面板，多个节点配置信息，只需添加相同格式的Nodes item即可。
``` yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # ./access.Log
  ErrorPath: # ./error.log
DnsConfigPath: # ./dns.json  Path to dns config
Nodes:
  -
    PanelType: "SSpanel" # Panel type: SSpanel
    ApiConfig:
      ApiHost: "http://127.0.0.1:667"
      ApiKey: "123"
      NodeID: 41
      NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request, Default is 5 sec
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: ./cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board
    ApiConfig:
      ApiHost: "http://V2board.com"
      ApiKey: "123"
      NodeID: 42
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node2.test.com" # Domain to cert
        CertFile: ./cert/node2.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node2.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```

## 配置文件设置说明

### 基础配置
基础配置是对所有节点生效的配置。
``` yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # ./access.Log
  ErrorPath: # ./error.log
DnsConfigPath: # ./dns.json  Path to dns config
```
#### 日志配置
日志配置用于控制XrayR-core的日志级别
``` yaml
Log:
  Level: debug # Log level: none, error, warning, info, debug 
  AccessPath: # ./access.Log
  ErrorPath: # ./error.log
```
| 参数         | 选项                                    | 说明                         |
| ------------ | --------------------------------------- | ---------------------------- |
| `Level`      | `none`,`error`,`warning`,`info`,`debug` | 日志显示级别，`none`为不显示 |
| `AccessPath` | 无                                      | Access日志的保存路径         |
| `ErrorPath`  | 无                                      | Error日志的保存路径          |

#### 自定义DNS配置

指定自定义DNS配置文件的路径

``` yaml
DnsConfigPath: # ./dns.json  Path to dns config
```
| 参数            | 选项 | 说明                    |
| --------------- | ---- | ----------------------- |
| `DnsConfigPath` | 无   | 自定义DNS配置文件的路径 |

### 节点配置

每个节点是一个独立的配置，互相不会影响，XrayR支持单实例多节点启动，同时对接多个节点。
``` yaml
Nodes:
  -
    PanelType: "SSpanel" # Panel type: SSpanel
    ApiConfig:
      ApiHost: "http://127.0.0.1:667"
      ApiKey: "123"
      NodeID: 41
      NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request, Default is 5 sec
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: ./cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
  -
    PanelType: "V2board" # Panel type: SSpanel, V2board
    ApiConfig:
      ApiHost: "http://V2board.com"
      ApiKey: "123"
      NodeID: 42
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 30 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type, Prefer remote configuration
      EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
    ControllerConfig:
      ListenIP: 0.0.0.0 # IP address you want to listen
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
      CertConfig:
        CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node2.test.com" # Domain to cert
        CertFile: ./cert/node2.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node2.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```
#### 面板选择
``` yaml
PanelType: "V2board" # Panel type: SSpanel, V2board
```
| 参数        | 选项                | 说明             |
| ----------- | ------------------- | ---------------- |
| `PanelType` | `SSPanel`,`V2board` | 对接前端面板类型 |

#### 面板对接配置
``` yaml
ApiConfig:
    ApiHost: "http://127.0.0.1:667"
    ApiKey: "123"
    NodeID: 41
    NodeType: V2ray # Node type: V2ray, Shadowsocks, Trojan
    Timeout: 30 # Timeout for the api request, Default is 5 sec
    EnableVless: false # Enable Vless for V2ray Type, Prefer remote
    EnableXTLS: false # Enable XTLS for V2ray and Trojan， Prefer remote configuration
```
| 参数          | 选项                           | 说明                                                          |
| ------------- | ------------------------------ | ------------------------------------------------------------- |
| `ApiHost`     | 无                             | 对接前端面板地址                                              |
| `ApiKey`      | 无                             | 前端对接通讯秘钥                                              |
| `NodeID`      | 无                             | 节点ID                                                        |
| `NodeType`    | `V2ray`,`Shadowsocks`,`Trojan` | 节点类型                                                      |
| `Timeout`     | 无                             | 设定单次访问API超时时间，默认5秒                              |
| `EnableVless` | `true`,`false`                 | 是否给V2ray启用Vless协议，SSP面板会优先使用远端配置覆盖此选项 |
| `EnableXTLS`  | `true`,`false`                 | 是否使用XTLS，SSP面板会优先使用远端配置覆盖此选项             |

#### 后端相关配置
``` yaml
ControllerConfig:
    ListenIP: 0.0.0.0 # IP address you want to listen
    UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
    EnableDNS: false # Enable custom DNS config, Please ensure that you set the dns.json well
```
| 参数         | 选项                                    | 说明                         |
| ------------ | --------------------------------------- | ---------------------------- |
| `ListenIP`      | 无| 选择监听的IP地址，`0.0.0.0`会同时监听v6和v4 |
|`UpdatePeriodic`|无|从前端更新节点、用户信息和上报用户使用信息的间隔，默认60秒|
|`EnableDNS`|`true`,`false`|是否为当前节点启用自定义DNS，默认使用系统DNS|

#### 证书申请相关配置
``` yaml
CertConfig:
    CertMode: dns # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
    CertDomain: "node2.test.com" # Domain to cert
    CertFile: ./cert/node2.test.com.cert # Provided if the CertMode is file
    KeyFile: ./cert/node2.test.com.key
    Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
    Email: test@me.com
    DNSEnv: # DNS ENV option used by DNS provider
        ALICLOUD_ACCESS_KEY: aaa
        ALICLOUD_SECRET_KEY: bbb
```
| 参数         | 选项                                    | 说明                         |
| ------------ | --------------------------------------- | ---------------------------- |
| `CertMode`      | `none`,`file`,`http`,`dns`| 获取证书的方式。`file`:手动提供，并制定路径。`http`：通过http申请，需要80端口。`dns`：使用dns模式申请，需要制定相关dns服务商配置。`none`：强制关闭tls设置，交由nginx或者caddy处理。|
|`CertDomain`|无|申请证书域名|
|`CertFile`|无|手动指定的证书路径|
|`KeyFile`|无|手动指定的私钥路径|
|`Provider`|无|dns提供商，所有支持的dns提供商请在此获取：https://go-acme.github.io/lego/dns/
|`DNSEnv`|无|采用DNS申请证书需要的环境变量，请参考上文链接内，自己的dns提供商所需要的参数，填写于此。请注意一行一个，填写时需符合yaml文件格式。|