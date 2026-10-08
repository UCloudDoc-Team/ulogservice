# 采集 SysLog 日志

SysLog 是一种网络协议，用于在网络中传递记录消息，通常被称为系统日志（System Log）。路由器、交换机、防火墙等网络设备，以及 Unix/Linux 服务器都普遍支持通过 SysLog 协议向外发送日志。本文将介绍如何在控制台配置 SysLog 采集规则，将 SysLog 日志投递到日志服务 ULogService。

## 使用场景

SysLog 记录了系统中每时每刻发生的事件，通过对 SysLog 的监控与管理，可以帮助企业减少系统停机时间、提高网络性能并加强安全策略。常见的采集场景包括：

- 网络设备日志：路由器、交换机、防火墙的运行日志、访问日志、安全事件日志。
- 主机系统日志：Unix/Linux 服务器的 `/var/log/messages`、`/var/log/syslog`、`/var/log/secure` 等系统日志。
- 应用运行日志：应用通过 SysLog 协议输出的运行日志与审计日志。

## 前提条件

1. 已开通日志服务 ULogService。
2. 已安装 ULogAgent，且 ULogAgent 版本支持 SysLog 采集。安装方式请参考 [LogAgent 安装指南（Linux 版）](/ulogservice/operate/logagent_install)。

## 日志样例

以一条常见的 RFC3164 格式 SysLog 为例：

```
<134>Sep  8 15:24:30 uhost-web-01 nginx: 192.168.1.1 - - [08/Sep/2025:15:24:30 +0800] "GET /index.html HTTP/1.1" 200 612
```

将**解析协议**配置为 `rfc3164` 后，该条日志最终被日志服务处理为如下字段：

| 字段 | 说明 |
| -- | -- |
| syslog.priority | 协议优先级，由 facility 与 severity 组合而成，如 `<134>` 对应 16 × 8 + 6。 |
| syslog.facility | 协议 facility 值，如 16 对应 local0。 |
| syslog.facility_label | facility 对应的名称，如 local0。 |
| event.severity | 协议 severity 值，如 6 对应 Info。 |
| syslog.severity_label | severity 对应的名称，如 Info。 |
| hostname | 日志中携带的主机名，如 uhost-web-01。 |
| process.program | 协议中的 tag / 程序名，如 nginx。 |
| message | 日志内容，即去除时间戳、主机名、tag 之后的剩余部分。 |
| log.source.address | 发送该条日志的来源地址与端口。 |

## 操作步骤

### 步骤1：进入日志接入

SysLog 采集配置需要关联到一个日志主题，从日志主题进入接入流程。

1. 登录 [ULogService 控制台](https://console.ucloud.cn/ulogservice/topic)。

2. 在**主题管理**页面，找到目标日志主题，单击该主题的**日志接入**按钮，在弹窗中选择 **SysLog 采集**。

![SysLog采集入口](/images/syslog/syslog_access_1.png)

### 步骤2：选择机器组

选择机器组，单击**下一步**。

![选择机器组](/images/syslog/syslog_machine_group_1.png)

> 说明：SysLog 采集依赖于目标机器上已安装并启动的 ULogAgent，请确保所选机器组中的机器与安装了 ULogAgent 的机器一致。

### 步骤3：SysLog 采集配置

完成机器组选择后，单击**下一步**进入采集配置，配置信息如下：

![SysLog采集配置](/images/syslog/syslog_collect_1.png)

| 配置项 | 类型 | 说明 |
| -- | -- | -- |
| 采集规则名称 | 输入框 | 本次采集规则的名称。 |
| 网络类型 | 单选框 | SysLog 的传输协议，可选 UDP 或 TCP，需与发送端（如 rsyslog）的转发配置保持一致。 |
| 解析协议 | 单选框 | 可选 `rfc3164`（RFC3164 格式）、`rfc5424`（RFC5424 格式）、`auto`（自动识别）。 |
| 监听地址 | 输入框 | 格式为 `[ip]:[port]`。<br>本机采集：使用 `127.0.0.1` 加上转发端口，例如 `127.0.0.1:514`。<br>其他服务器采集：填写**安装了 ULogAgent 的机器 IP**，且需与 rsyslog 的转发地址、端口一致，配置方式请参考**使用 rsyslog 转发日志**。 |
| 上传解析失败日志 | 开关 | 开启：解析失败的日志以指定的键名称（Key）作为字段名，将原始日志内容整体作为字段值上传到日志服务。<br>关闭：解析失败的日志将被丢弃。 |
| 解析失败日志的键名称（Key） | 输入框 | 解析失败日志所使用的字段名，默认为 `LogParseFailure`。 |

### 步骤4：索引配置

采集配置完成后，单击**完成创建**，进入**索引配置**页面，根据业务需求设置索引配置信息。

![索引配置](/images/syslog/syslog_index_1.png)

> 注意：
> - 日志检索必须先开启索引配置，否则无法检索日志。
> - 修改索引规则只对新增写入的日志生效，已写入的历史数据不会同步更新。

配置完成后单击**确定**，即可完成 SysLog 日志采集配置。

## 使用 rsyslog 转发日志

主机上的系统日志需要借助 rsyslog 等转发工具上报：由产生日志的机器将 SysLog 转发到安装了 ULogAgent 的机器，再由 ULogAgent 完成采集与上报（网络设备可直接将 SysLog 发送到 ULogAgent 的监听地址）。根据产生日志的机器与安装 ULogAgent 的机器是否为同一台，分为以下两种方式。

### 本机采集

若产生日志的机器与安装 ULogAgent 的机器是同一台，可按如下方式配置。

编辑 `/etc/rsyslog.conf`，在文件末尾追加一条转发规则，rsyslog 会将 SysLog 转发到指定的 IP 和端口。

```
*.* @@127.0.0.1:514
```

> 说明：
> - `*.*` 表示转发所有 facility、所有 severity 的日志，可按需调整。
> - `@@` 表示使用 TCP 协议转发，`@` 表示使用 UDP 协议转发，请与采集配置中的**网络类型**保持一致。
> - `127.0.0.1:514` 为本机的转发地址和端口，514 为 SysLog 的默认端口。

![rsyslog转发配置](/images/syslog/syslog_rsyslog_1.png)

执行如下命令重启 rsyslog，使配置生效：

```
systemctl restart rsyslog
```

### 其他服务器采集

若产生日志的机器与安装 ULogAgent 的机器不是同一台，则在**产生日志的机器**上配置 rsyslog 转发规则，转发地址填写**安装了 ULogAgent 的机器 IP**。假设 ULogAgent 所在机器的 IP 为 `192.168.1.100`，则在 `/etc/rsyslog.conf` 末尾追加：

```
*.* @@192.168.1.100:514
```

配置完成后，同样执行 `systemctl restart rsyslog` 使配置生效。转发方式（`@@` 或 `@`）需与采集配置中的**网络类型**保持一致。
