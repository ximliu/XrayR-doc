# 设备连接限制功能说明

由于大量面板不再支持远程设备限制指定，现增加本地设备限制参数。

如需启用，可在配置文件中将`DeviceLimit`设为非0值，注意此设置会覆盖远程获取的用户设备限制数目。

配置文件详见：[配置文件说明](../xrayr-pei-zhi-wen-jian-shuo-ming/config.md#mian-ban-dui-jie-pei-zhi)

## 全局设备限制

当XrayR版本&gt;=v0.7.1，SSpanel版本&gt;=[2021.9](https://github.com/Anankke/SSPanel-Uim/releases/tag/2021.9)，XrayR将会针对SSpanel启用全局设备限制功能。此时，不同后端结点将会全局限制独立IP连接数量，而非各后端本地限制。

