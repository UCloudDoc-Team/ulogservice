# 聚合函数

聚合函数用于对一组值进行计算并返回单个值，常与 `GROUP BY` 子句配合使用，实现分组统计功能。

## 函数列表

| 函数名称 | 语法 | 说明 |
|---------|------|------|
| arbitrary函数 | `arbitrary(x)` | 返回 x 中任意一个非空的值。 |
| avg函数 | `avg(x)` | 计算 x 的算术平均值。 |
| bitwise_and_agg函数 | `bitwise_and_agg(x, y)` | 返回 x 和 y 按位与运算（AND）的结果。 |
| bitwise_or_agg函数 | `bitwise_or_agg(x, y)` | 返回 x 和 y 按位或运算（OR）的结果。 |
| checksum函数 | `checksum(x)` | 计算 x 的校验和。 |
| count函数 | `count(*)` | 统计所有的日志条数。 |
| | `count(1)` | 统计所有的日志条数，等同于 count(*)。 |
| | `count(x)` | 统计 x 中值不为 NULL 的日志条数。 |
| max函数 | `max(x)` | 查询 x 中的最大值。 |
| max_by函数 | `max_by(x, y)` | 查询 y 为最大值时对应的 x 值。 |
| min函数 | `min(x)` | 查询 x 中最小值。 |
| min_by函数 | `min_by(x, y)` | 查询 y 为最小值时对应的 x 值。 |
| sum函数 | `sum(x)` | 计算 x 的总值。 |

## 函数说明

### arbitrary

返回 x 中任意一个非空的值。

**语法**：
```sql
arbitrary(x)
```

**示例**：
```sql
-- 获取任意一个请求路径
SELECT arbitrary(path) as sample_path

-- 按状态码分组获取任意一条日志的路径
SELECT status, arbitrary(path) as sample_path
GROUP BY status
```

### avg

计算 x 的算术平均值。

**语法**：
```sql
avg(x)
```

**示例**：
```sql
-- 计算平均响应时间
SELECT avg(response_time) as avg_time

-- 按状态码统计平均响应时间
SELECT status, avg(response_time) as avg_time
GROUP BY status
```

### bitwise_and_agg

返回 x 和 y 按位与运算（AND）的结果。

**语法**：
```sql
bitwise_and_agg(x, y)
```

**示例**：
```sql
-- 计算两个权限标志的按位与结果
SELECT bitwise_and_agg(perm1, perm2) as combined_permissions
```

### bitwise_or_agg

返回 x 和 y 按位或运算（OR）的结果。

**语法**：
```sql
bitwise_or_agg(x, y)
```

**示例**：
```sql
-- 计算两个权限标志的按位或结果
SELECT bitwise_or_agg(perm1, perm2) as total_permissions
```

### checksum

计算 x 的校验和，用于数据完整性校验。

**语法**：
```sql
checksum(x)
```

**示例**：
```sql
-- 计算字段的校验和
SELECT checksum(message) as msg_checksum

-- 按日期计算校验和
SELECT date(__time__) as log_date, checksum(content) as content_checksum
GROUP BY log_date
```

### count

统计日志条数。

**语法**：
```sql
count(*)    -- 统计所有的日志条数
count(1)    -- 统计所有的日志条数，等同于 count(*)
count(x)    -- 统计 x 中值不为 NULL 的日志条数
```

**示例**：
```sql
-- 统计日志总条数
SELECT count(*) as total

-- 统计有 status 字段值的日志条数
SELECT count(status) as status_count

-- 按状态码分组统计
SELECT status, count(*) as count
GROUP BY status
```

### max

查询 x 中的最大值。

**语法**：
```sql
max(x)
```

**示例**：
```sql
-- 获取最大响应时间
SELECT max(response_time) as max_time

-- 按请求方法获取最大响应时间
SELECT method, max(response_time) as max_time
GROUP BY method
```

### max_by

查询 y 为最大值时对应的 x 值。

**语法**：
```sql
max_by(x, y)
```

**示例**：
```sql
-- 获取响应时间最长的请求路径
SELECT max_by(path, response_time) as slowest_path

-- 按日期获取响应时间最长的请求
SELECT
  date(__time__) as log_date,
  max_by(path, response_time) as slowest_path,
  max(response_time) as max_time
GROUP BY log_date
```

### min

查询 x 中最小值。

**语法**：
```sql
min(x)
```

**示例**：
```sql
-- 获取最小响应时间
SELECT min(response_time) as min_time

-- 按请求方法获取最小响应时间
SELECT method, min(response_time) as min_time
GROUP BY method
```

### min_by

查询 y 为最小值时对应的 x 值。

**语法**：
```sql
min_by(x, y)
```

**示例**：
```sql
-- 获取响应时间最短的请求路径
SELECT min_by(path, response_time) as fastest_path

-- 按日期获取响应时间最短的请求
SELECT
  date(__time__) as log_date,
  min_by(path, response_time) as fastest_path,
  min(response_time) as min_time
GROUP BY log_date
```

### sum

计算 x 的总值。

**语法**：
```sql
sum(x)
```

**示例**：
```sql
-- 计算总响应时间
SELECT sum(response_time) as total_time

-- 按请求方法统计总响应时间
SELECT method, sum(response_time) as total_time
GROUP BY method
```

## 使用示例

### 综合统计

```sql
-- 综合统计请求情况
SELECT
  count(*) as total_requests,
  avg(response_time) as avg_time,
  max(response_time) as max_time,
  min(response_time) as min_time
```

### 分组聚合

```sql
-- 按状态码分组统计
SELECT
  status,
  count(*) as count,
  avg(response_time) as avg_time,
  sum(bytes_sent) as total_bytes
GROUP BY status
ORDER BY count DESC
```

### 极值查询

```sql
-- 查询响应时间最长和最短的请求
SELECT
  max_by(path, response_time) as slowest_path,
  max(response_time) as max_time,
  min_by(path, response_time) as fastest_path,
  min(response_time) as min_time
```

### 按状态码分组统计

```sql
-- 按状态码分组统计请求数量
SELECT
  method,
  status,
  count(*) as count
GROUP BY method, status
ORDER BY method, status
```
