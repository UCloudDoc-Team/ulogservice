# URL 函数

URL 函数用于解析和处理 URL 字符串，可以提取 URL 的各个组成部分，以及进行 URL 编码解码操作。

## 函数列表

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| url_encode函数 | `url_encode(x)` | 对 URL 进行编码。 |
| url_decode函数 | `url_decode(x)` | 对 URL 进行解码。 |
| url_extract_fragment函数 | `url_extract_fragment(x)` | 从 URL 中提取 Fragment 信息。 |
| url_extract_host函数 | `url_extract_host(x)` | 从 URL 中提取 Host 信息。 |
| url_extract_parameter函数 | `url_extract_parameter(x, parameter name)` | 从 URL 的查询部分中提取指定参数的值。 |
| url_extract_path函数 | `url_extract_path(x)` | 从 URL 中提取访问路径信息。 |
| url_extract_protocol函数 | `url_extract_protocol(x)` | 从 URL 中提取协议信息。 |
| url_extract_query函数 | `url_extract_query(x)` | 从 URL 中提取查询部分的信息。 |

## 函数说明

### url_encode

对 URL 进行编码，将特殊字符转换为 URL 安全格式。

**语法**：
```sql
url_encode(x)
```

**示例**：
```sql
-- 对 URL 进行编码
SELECT url_encode('https://example.com/search?q=你好')
-- 返回: https%3A%2F%2Fexample.com%2Fsearch%3Fq%3D%E4%BD%A0%E5%A5%BD
```

### url_decode

对 URL 进行解码，将编码后的字符还原。

**语法**：
```sql
url_decode(x)
```

**示例**：
```sql
-- 对 URL 进行解码
SELECT url_decode('https%3A%2F%2Fexample.com%2Fsearch%3Fq%3Dhello')
-- 返回: https://example.com/search?q=hello
```

### url_extract_protocol

从 URL 中提取协议信息。

**语法**：
```sql
url_extract_protocol(x)
```

**示例**：
```sql
-- 提取协议
SELECT url_extract_protocol('https://www.example.com:8080/path?query=1')
-- 返回: https

-- 统计不同协议的使用情况
SELECT
  url_extract_protocol(url) as protocol,
  count(*) as count
GROUP BY protocol
```

### url_extract_host

从 URL 中提取 Host 信息。

**语法**：
```sql
url_extract_host(x)
```

**示例**：
```sql
-- 提取主机名
SELECT url_extract_host('https://www.example.com:8080/path')
-- 返回: www.example.com

-- 统计各域名的访问量
SELECT
  url_extract_host(url) as host,
  count(*) as count
GROUP BY host
ORDER BY count DESC
```

### url_extract_path

从 URL 中提取访问路径信息。

**语法**：
```sql
url_extract_path(x)
```

**示例**：
```sql
-- 提取路径
SELECT url_extract_path('https://www.example.com/api/v1/users?id=1')
-- 返回: /api/v1/users

-- 统计各路径的访问量
SELECT
  url_extract_path(url) as path,
  count(*) as count
GROUP BY path
ORDER BY count DESC
LIMIT 10
```

### url_extract_query

从 URL 中提取查询部分的信息。

**语法**：
```sql
url_extract_query(x)
```

**示例**：
```sql
-- 提取查询字符串
SELECT url_extract_query('https://example.com/search?q=hello&page=1')
-- 返回: q=hello&page=1
```

### url_extract_parameter

从 URL 的查询部分中提取指定参数的值。

**语法**：
```sql
url_extract_parameter(x, parameter name)
```

**示例**：
```sql
-- 提取指定参数
SELECT url_extract_parameter('https://example.com/search?q=hello&page=1', 'q')
-- 返回: hello

SELECT url_extract_parameter('https://example.com/search?q=hello&page=1', 'page')
-- 返回: 1

-- 统计各搜索关键词
SELECT
  url_extract_parameter(url, 'q') as keyword,
  count(*) as count
WHERE url_extract_path(url) = '/search'
GROUP BY keyword
ORDER BY count DESC
LIMIT 10
```

### url_extract_fragment

从 URL 中提取 Fragment 信息。

**语法**：
```sql
url_extract_fragment(x)
```

**示例**：
```sql
-- 提取 Fragment
SELECT url_extract_fragment('https://example.com/page#section1')
-- 返回: section1
```

## 使用示例

### URL 结构分析

```sql
-- 解析 URL 各部分
SELECT
  url,
  url_extract_protocol(url) as protocol,
  url_extract_host(url) as host,
  url_extract_path(url) as path,
  url_extract_query(url) as query
LIMIT 10
```

### API 接口访问统计

```sql
-- 统计各 API 路径的访问量
SELECT
  url_extract_path(url) as api_path,
  count(*) as requests,
  avg(response_time) as avg_time
GROUP BY api_path
ORDER BY requests DESC
LIMIT 20
```

### 查询参数分析

```sql
-- 分析分页参数分布
SELECT
  url_extract_parameter(url, 'page') as page,
  count(*) as count
WHERE url_extract_path(url) = '/api/list'
GROUP BY page
ORDER BY count DESC
```

### 域名访问分析

```sql
-- 统计各域名及协议的请求量
SELECT
  url_extract_protocol(url) as protocol,
  url_extract_host(url) as host,
  count(*) as requests
GROUP BY protocol, host
ORDER BY requests DESC
```
