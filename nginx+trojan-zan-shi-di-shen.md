# Nginx+Trojan暂时滴神！

使用Nginx处理Trojan的TLS，Trojan进行回落。我愿称他暂时滴神！


## Nginx安装

<p>CentOS：<br />
apt update<br />
yum install -y nginx<br />
yum install nginx-mod-stream</p>

<p>Ubuntu:<br />
apt update<br />
apt install nginx</p>



## Nginx配置

修改/etc/nginx/nginx.conf配置文件：

```text
stream {
    server {
        listen              443 ssl;                    # 设置监听端口为443

        ssl_protocols       TLSv1.2 TLSv1.3;      # 设置使用的SSL协议版本
        
        ssl_certificate /etc/nginx/ssl/xx.com.pem; # 证书地址
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # 秘钥地址
        ssl_session_cache   shared:SSL:10m;             # SSL TCP会话缓存设置共享内存区域名为
                                                        # SSL，区域大小为10MB
        ssl_session_timeout 10m;                        # SSL TCP会话缓存超时时间为10分钟
        proxy_protocol    on; # 开启proxy_protocol获取真实ip
        proxy_pass        127.0.0.1:1234; # 后端Trojan监听端口
    }
}
```
<p>请将上方代码添加到<strong>http</strong>与<strong>events</strong>中间一行</p>

**/etc/nginx/nginx.conf配置文件参考：**

```text
events {
	worker_connections 768;
	# multi_accept on;
}

stream {
    server {
        listen              443 ssl;                    # 设置监听端口为443

        ssl_protocols       TLSv1.2 TLSv1.3;      # 设置使用的SSL协议版本
        
        ssl_certificate /etc/nginx/ssl/xx.com.pem; # 证书地址
        ssl_certificate_key /etc/nginx/ssl/xx.com.key; # 秘钥地址
        ssl_session_cache   shared:SSL:10m;             # SSL TCP会话缓存设置共享内存区域名为
                                                        # SSL，区域大小为10MB
        ssl_session_timeout 10m;                        # SSL TCP会话缓存超时时间为10分钟
        proxy_protocol    on; # 开启proxy_protocol获取真实ip
        proxy_pass        127.0.0.1:1234; # 后端Trojan监听端口
    }
}

http {

	##
	# Basic Settings
	##
```

<p><strong>注意事项：</strong></p>

<p><strong>1. 请配置SSL证书</strong></p>

<p><strong>2. proxy_pass 127.0.0.1:1234 后端Trojan监听端口与您网站前端节点监听端口一致</strong></p>

<p><strong>3. listen端口可以1-65535随意修改，此处为客户端连接端口</strong></p>


## XrayR Trojan配置

**关键配置：**

```text
ListenIP: 127.0.0.1
EnableProxyProtocol: true
EnableFallback: true
CertMode: none
```

{% hint style="info" %}
注意1：请务必确保CertMode为none，交由Nginx处理tls
{% endhint %}

{% hint style="info" %}
注意2：在回落时请确保回落站点是http1.1，nginx如果有一个站点是h2会导致全部站点都变成h2（巨坑）
{% endhint %}

**完整样例**

```text
  -
    PanelType: "SSpanel" # Panel type: SSpanel, V2board, PMpanel
    ApiConfig:
      ApiHost: "https://xxx.com"
      ApiKey: "123"
      NodeID: 1
      NodeType: Trojan # Node type: V2ray, Shadowsocks, Trojan
      Timeout: 10 # Timeout for the api request
      EnableVless: false # Enable Vless for V2ray Type
      EnableXTLS: false # Enable XTLS for V2ray and Trojan
      SpeedLimit: 0 # Mbps, Local settings will replace remote settings, 0 means disable
      DeviceLimit: 0 # Local settings will replace remote settings, 0 means disable
      RuleListPath: # ./rulelist Path to local rulelist file
    ControllerConfig:
      ListenIP: 127.0.0.1 # IP address you want to listen
      SendIP: 0.0.0.0 # IP address you want to send pacakage
      UpdatePeriodic: 60 # Time to update the nodeinfo, how many sec.
      EnableDNS: false # Use custom DNS config, Please ensure that you set the dns.json well
      DNSType: AsIs # AsIs, UseIP, UseIPv4, UseIPv6, DNS strategy
      EnableProxyProtocol: true # Only works for WebSocket and TCP
      EnableFallback: true # Only support for Trojan and Vless
      FallBackConfigs:  # Support multiple fallbacks
        -
          SNI: # TLS SNI(Server Name Indication), Empty for any
          Path: # HTTP PATH, Empty for any
          Dest: fake.website.com:80 # Required, Destination of fallback, check https://xtls.github.io/config/fallback/ for details.
          ProxyProtocolVer: 0 # Send PROXY protocol version, 0 for dsable
      CertConfig:
        CertMode: none # Option about how to get certificate: none, file, http, dns. Choose "none" will forcedly disable the tls config.
        CertDomain: "node1.test.com" # Domain to cert
        CertFile: ./cert/node1.test.com.cert # Provided if the CertMode is file
        KeyFile: ./cert/node1.test.com.key
        Provider: alidns # DNS cert provider, Get the full support list here: https://go-acme.github.io/lego/dns/
        Email: test@me.com
        DNSEnv: # DNS ENV option used by DNS provider
          ALICLOUD_ACCESS_KEY: aaa
          ALICLOUD_SECRET_KEY: bbb
```


## 重启并检查 Nginx 和 XrayR

<p>systemctl restart nginx<br />
XrayR restart</p>

<p>systemctl status nginx<br />
XrayR restart</p>


