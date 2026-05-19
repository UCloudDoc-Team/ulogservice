# 逻辑运算符

逻辑运算符用于组合多个条件表达式，支持 AND、OR、NOT 三种基本逻辑运算。

## 运算符列表

| 运算符 | 语法 | 说明 |
|--------|------|------|
| AND | `条件1 AND 条件2` | 两个条件都为真时返回 true |
| OR | `条件1 OR 条件2` | 任一条件为真时返回 true |
| NOT | `NOT 条件` | 对条件取反 |

## 运算符说明

### AND

当两个条件都为真时，返回 true。

**语法**：
```sql
条件1 AND 条件2
```

**示例**：
```sql
-- 筛选状态码为 200 且响应时间大于 100ms 的日志
SELECT *
WHERE status = 200 AND response_time > 100

-- 筛选特定 IP 和状态码的日志
SELECT *
WHERE client_ip = '192.168.1.1' AND status = 404
```

### OR

当任一条件为真时，返回 true。

**语法**：
```sql
条件1 OR 条件2
```

**示例**：
```sql
-- 筛选状态码为 404 或 500 的日志
SELECT *
WHERE status = 404 OR status = 500

-- 筛选特定请求方法的日志
SELECT *
WHERE method = 'GET' OR method = 'POST'
```

### NOT

对条件取反，当原条件为假时返回 true。

**语法**：
```sql
NOT 条件
```

**示例**：
```sql
-- 筛选状态码不是 200 的日志
SELECT *
WHERE NOT status = 200

-- 筛选非 GET 请求的日志
SELECT *
WHERE NOT method = 'GET'
```

## 运算符优先级

逻辑运算符的优先级从高到低为：NOT > AND > OR。可以使用括号改变优先级。

**示例**：
```sql
-- 优先计算 AND，再计算 OR
-- 等价于：(status = 200 AND method = 'GET') OR status = 500
SELECT *
WHERE status = 200 AND method = 'GET' OR status = 500

-- 使用括号改变优先级
SELECT *
WHERE status = 200 AND (method = 'GET' OR method = 'POST')
```

## 使用示例

### 多条件组合

```sql
-- 筛选错误日志（状态码 4xx 或 5xx）且响应时间大于 1s
SELECT *
WHERE (status >= 400 AND status < 500) OR (status >= 500 AND response_time > 1000)
```

### 复杂逻辑查询

```sql
-- 筛选成功请求但不包含特定路径
SELECT *
WHERE status = 200 AND NOT path LIKE '/health%'
```

### 分组统计配合逻辑运算

```sql
-- 按请求方法统计错误日志
SELECT
  method,
  COUNT(*) as error_count
WHERE status >= 400
GROUP BY method
ORDER BY error_count DESC
```
