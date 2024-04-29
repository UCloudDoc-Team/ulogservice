# 日志采集

日志服务（ULS）使用 Filebeat 组件采集日志并导入到日志服务。

## 安装启动

### 1. 下载Filebeat

下载对应操作系统版本的 [Filebeat](https://www.elastic.co/cn/downloads/past-releases/filebeat-7-10-2)

以安装路径`/usr/local/`为例：Filebeat 解压路径为`/usr/local/`，解压完成后进入 Filebeat 目录`/usr/local/filebeat-${version}`。

### 2. 配置采集日志

编辑filebeat.yml配置文件

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - #{log_path}

output.kafka:
  hosts: ["kafka1:9092", "kafka2:9092", "kafka3:9092"]
  topic: '#{topic_name}'
```

> 注：
>
> - #{log_path} 采集日志目录文件
> - hosts 为接入地址
> - #{topic_name} 日志主题

### 3. 启动Filebeat

**启动命令** ./filebeat run -c filebeat.yml

