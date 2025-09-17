# 单行全文提取模式采集日志

## 背景信息

单行文本日志表示一行日志即为一条日志，换行符（\n）为一条日志结束的标识符。
如果无需对日志内容进行结构化处理、无需提取日志字段进行精细化分析查询，建议使用单行全文模式。

## 日志样例

常见的单行日志样例如下：
```
123.123.123.123 - - [17/Sep/2025:15:24:30 +0800] "GET /index.html HTTP/1.1" 200 612 "https://example.com/referer" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36" "-"
```
日志最终被日志服务处理为：
```
"message": "123.123.123.123 - - [17/Sep/2025:15:24:30 +0800] \"GET /index.html HTTP/1.1\" 200 612 \"https://example.com/referer\" \"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36\" \"-\""
```

## 操作步骤

### 步骤1：创建/选择日志主题
参考主题、主题集管理

### 步骤2：采集配置

#### 配置日志文件采集路径
![文本路径](/images/text/text_path_1.png)
**文件路径**即日志所在的目录和文件名，ULogAgent 会按照采集路径中的目录部分匹配符合规则的目录，监听这些目录下符合规则的日志文件。最多设置 10 个不同的采集路径。
采集路径可以指定完整的目录和文件名，也可以通过通配符模糊匹配。

常见的采集路径的配置方式及示例如下。

| 配置方式       | 目录前缀表达式 | 文件名表达式 | 说明                                                         |
| -------------- | -------------- | ------------ | ------------------------------------------------------------ |
| 完整文件名称   | /var/log/nginx | access.log   | 此例中，日志路径配置为`/var/log/nginx/**/access.log`，LogListener 将会监听`/var/log/nginx`前缀路径下所有子目录中以`access.log`命名的日志文件 |
| 文件名后缀匹配 | /var/log/nginx | *.log        | 此例中，日志路径配置为`/var/log/nginx/**/*.log`，LogListener 将会监听`/var/log/nginx`前缀路径下所有子目录中以`.log`结尾的日志文件 |
| 文件名模糊匹配 | /var/log/nginx | error*       | 此例中，日志路径配置为`/var/log/nginx/**/error*`，LogListener 将会监听`/var/log/nginx`前缀路径下所有子目录中以`error`开头命名的日志文件 |

#### 配置采集策略
- 全量：ULogAgent 采集文件时，从文件的开头开始读。
- 增量：ULogAgent 采集文件时，只采集文件内新增的内容。

#### 配置编码模式
- UTF-8：若您的日志文件编码模式为 UTF-8，请选择该选项。
- GBK：若您的日志文件编码模式为 GBK，请选择该选项。

#### 配置单行全文格式
在“采集配置”页面，将“提取模式”设置为单行全文。如下图所示：
![文本路径](/images/text/text_minimal_line_1.png)
确认采集配置，并单击完成创建。

### 步骤3：索引配置
![文本路径](/images/text/text_index_1.png)