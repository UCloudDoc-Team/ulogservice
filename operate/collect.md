# 日志采集

日志服务（ULogService）使用 UCloud 定制 Filebeat 组件采集日志并导入到日志服务。

## 安装启动

### 1. 下载Filebeat

LUNIX X86_64 系列
```
wget https://ulogservice.cn-wlcb.ufileos.com/download/filebeat/filebeat-uls-7.12.21-linux-x86_64.tar.gz
```



### 2. 配置采集日志

编辑filebeat.yml配置文件

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - #{log_path}

output.uls:
  enabled: true
  endpoint: internal.cn-wlcb.uls.ucloud.cn
  protocol: http
  topic_id: #{topic_name}
  public_key: #{public_key}
  private_key: #{private_key}
  format: json
```

> 注：
>
> - #{log_path} 采集日志目录文件
> - endpoint 为接入地址
> - #{topic_name} 日志主题
> - #{public_key} 令牌公钥
> - #{private_key} 令牌私钥


### 3. 启动Filebeat

**示例：** 启动命令 

```
./filebeat -c filebeat.yml
```



## 地域和域名

日志服务提供内网域名访问方式：内网域名可响应来自于同一地域下的 UCloud 公有云服务的访问请求（如：UHost 云主机、UK8S 容器云等）。

**公有云下日志服务地域访问域名（Endpoint）如下：**


| 地域   | 内网域名                       |
| ------ | ------------------------------ |
| 华北二 | internal.cn-wlcb.uls.ucloud.cn |
