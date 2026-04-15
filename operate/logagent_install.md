# LogAgent安装指南（Linux 版）
LogAgent 是日志服务（ULS）提供的日志采集客户端，用于快速上报日志数据。在云主机（UHost）中安装LogAgent之后，通过控制台即可下发采集配置、接入日志服务。如果是UK8S接入日志服务，不需要手动安装LogAgent，可直接在控制台上自动安装。

## 注意事项
* 目前LogAgent只支持Linux x86_64云主机
* 云主机上需要安装 systemd 和 wget 程序
* 需要具备root权限来完成LogAgent的安装
* 确定地域信息，比如华北（北京2）地域为：cn-bj2。支持的地域请参考：[地域和访问域名](/ulogservice/region_domain_name)

## 下载LogAgent
LogAgent文件保存在US3上，可以选择对应地域的地址进行下载。推荐使用内网下载地址，下载速度更快。

| 地域 | 内网地址 | 外网地址 |
| -- | -- | -- |
| 华北（北京2）| http://uls-logagent-cn-bj.ufile.cn-north-02.ucloud.cn/logagent.tar.gz | http://uls-logagent-cn-bj.cn-bj.ufileos.com/logagent.tar.gz |
| 华北（乌兰察布）| http://uls-logagent-cn-wlcb.internal-cn-wlcb.ufileos.com/logagent.tar.gz | http://uls-logagent-cn-wlcb.cn-wlcb.ufileos.com/logagent.tar.gz |
| 俄罗斯（莫斯科） | http://uls-logagent-rus-mosc.internal-rus-mosc.ufileos.com/logagent.tar.gz | http://uls-logagent-rus-mosc.rus-mosc.ufileos.com/logagent.tar.gz |

使用如下命令安装：
```
wget $url && tar zxvf logagent.tar.gz -C /usr/local/
```
比如云主机在华北（乌兰察布），那么就可以使用如下命令（内网）：
```
wget http://uls-logagent-cn-wlcb.internal-cn-wlcb.ufileos.com/logagent.tar.gz && tar zxvf logagent.tar.gz -C /usr/local/
```

## 安装并启动LogAgent
### 1. 安装LogAgent
执行以下命令安装LogAgent。
```
cd /usr/local/logagent/ && ./logagent.sh region public_key private_key project_id
```
上述四个参数都是必填项，详细说明如下：

| 参数 | 实例 | 说明 |
| -- | -- | -- |
| region | cn-bj2 | LogAgent所在的地域，日志服务地域对应缩写请参考[地域和访问域名](/ulogservice/region_domain_name) |
| public_key | 4eZ****** | UCloud 账号管理里API密钥的公钥 |
| private_key | Fm8****** | UCloud 账号管理里API密钥的私钥 |
| project_id | org-****** | 日志主题所在的项目ID |

### 2. 设置机器标识
根据机器组的关联方式，选择对应的操作：
* 如果机器组使用IP地址：无需设置机器标识，可跳过当前步骤，参考：[机器组管理](/ulogservice/resource/machinegroup)完成配置
* 如果机器组使用机器标识：需要手动修改yml文件，在/usr/local/logagent/filebeat.yml文件的ulogservice下中增加标识列表。
示例：增加http和nginx两个标识，配置如下：
```
ulogservice:
    group_labels:
        - http
        - nginx
```


### 3. 启动LogAgent
执行以下命令启动LogAgent：
```
systemctl start logagent
```
使用如下命令观察LogAgent状态：
```
systemctl status logagent
```
加上开机自启动：
```
systemctl enable logagent
```
