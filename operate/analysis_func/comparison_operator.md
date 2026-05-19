# 比较运算符

比较运算符用于比较两个值，返回布尔值结果（true 或 false），常用于 WHERE 子句的筛选条件。

## 运算符列表

| 运算符 | 语法 | 说明 |
|--------|------|------|
| 基础运算符 | `x < y` | x 小于 y 时，返回 true。 |
| | `x > y` | x 大于 y 时，返回 true。 |
| | `x <= y` | x 小于或等于 y 时，返回 true。 |
| | `x >= y` | x 大于或等于 y 时，返回 true。 |
| | `x = y` | x 等于 y 时，返回 true。 |
| | `x <> y` | x 不等于 y 时，返回 true。 |
| | `x != y` | x 不等于 y 时，返回 true。 |
| BETWEEN运算符 | `x BETWEEN y AND z` | x 处在 y 和 z 之间时，返回 true。 |
| DISTINCT运算符 | `x IS DISTINCT FROM y` | x 不等于 y 时，返回 true。 |
| | `x IS NOT DISTINCT FROM y` | x 等于 y 时，返回 true。 |
| LIKE运算符 | `x LIKE pattern [escape 'escape_character']` | 用于匹配字符串中指定的字符模式。字符串区分大小写。 |
| GREATEST运算符 | `GREATEST(x, y...)` | 查询 x、y 中的最大值。 |
| LEAST运算符 | `LEAST(x, y...)` | 查询 x、y 中的最小值。 |
| NULL运算符 | `x IS NULL` | x 为 null 时，返回 true。 |
| | `x IS NOT NULL` | x 不为 null 时，返回 true。 |

## 运算符说明

### 基础运算符

基础比较运算符用于比较两个值的大小关系。

**语法**：
```sql
x < y    -- 小于
x > y    -- 大于
x <= y   -- 小于等于
x >= y   -- 大于等于
x = y    -- 等于
x <> y   -- 不等于
x != y   -- 不等于
```

**示例**：
```sql
-- 筛选状态码为 200 的日志
SELECT *
WHERE status = 200

-- 筛选响应时间大于 1000ms 的日志
SELECT *
WHERE response_time > 1000

-- 筛选状态码小于 400 的日志
SELECT *
WHERE status < 400

-- 筛选状态码不为 200 的日志
SELECT *
WHERE status != 200

-- 使用 <> 语法
SELECT *
WHERE status <> 200
```

### BETWEEN 运算符

判断值是否在指定范围内，包含边界值。

**语法**：
```sql
x BETWEEN y AND z
```

**示例**：
```sql
-- 筛选响应时间在 100ms 到 500ms 之间的日志
SELECT *
WHERE response_time BETWEEN 100 AND 500

-- 筛选特定时间段（使用时间戳）
SELECT *
WHERE __time__ BETWEEN 1704067200 AND 1705276800
```

### DISTINCT 运算符

用于比较两个值是否相等，与普通比较运算符的区别在于可以正确处理 NULL 值。

**语法**：
```sql
x IS DISTINCT FROM y      -- x 不等于 y 时返回 true
x IS NOT DISTINCT FROM y  -- x 等于 y 时返回 true
```

**示例**：
```sql
-- IS DISTINCT FROM 可以正确处理 NULL
-- 普通的 x <> y 在 x 或 y 为 NULL 时返回 NULL
-- IS DISTINCT FROM 在 x 或 y 为 NULL 时也能返回布尔值

SELECT *
WHERE status IS DISTINCT FROM 200

-- IS NOT DISTINCT FROM 用于判断相等（包括 NULL = NULL 的情况）
SELECT *
WHERE user_id IS NOT DISTINCT FROM NULL
```

### LIKE 运算符

用于字符串模糊匹配，支持通配符：
- `%`：匹配任意长度的字符串
- `_`：匹配单个字符

**语法**：
```sql
x LIKE pattern [escape 'escape_character']
```

**示例**：
```sql
-- 匹配以 /api 开头的路径
SELECT *
WHERE path LIKE '/api%'

-- 匹配包含 .jpg 的路径
SELECT *
WHERE path LIKE '%.jpg%'

-- 匹配第三个字符为 a 的字符串
SELECT *
WHERE path LIKE '__a%'

-- 使用 escape 转义特殊字符
SELECT *
WHERE path LIKE '/api/%/test' escape '/'
```

### GREATEST 运算符

返回多个值中的最大值。

**语法**：
```sql
GREATEST(x, y...)
```

**示例**：
```sql
-- 获取多个响应时间中的最大值
SELECT GREATEST(response_time1, response_time2, response_time3) as max_time

-- 获取多个数值字段的最大值
SELECT GREATEST(cpu_usage, memory_usage, disk_usage) as max_usage
```

### LEAST 运算符

返回多个值中的最小值。

**语法**：
```sql
LEAST(x, y...)
```

**示例**：
```sql
-- 获取多个响应时间中的最小值
SELECT LEAST(response_time1, response_time2, response_time3) as min_time

-- 获取多个数值字段的最小值
SELECT LEAST(cpu_usage, memory_usage, disk_usage) as min_usage
```

### NULL 运算符

判断字段值是否为空。

**语法**：
```sql
x IS NULL      -- x 为 null 时返回 true
x IS NOT NULL  -- x 不为 null 时返回 true
```

**示例**：
```sql
-- 筛选没有 user_id 字段的日志
SELECT *
WHERE user_id IS NULL

-- 筛选有 user_id 字段的日志
SELECT *
WHERE user_id IS NOT NULL
```

## 使用示例

### 多条件筛选

```sql
-- 筛选状态码为 200 且响应时间在 100-500ms 之间的 GET 请求
SELECT *
WHERE status = 200
  AND response_time BETWEEN 100 AND 500
  AND method = 'GET'
```

### 模糊查询

```sql
-- 查询路径以 /api/v1 开头的日志
SELECT path, COUNT(*) as count
WHERE path LIKE '/api/v1%'
GROUP BY path
ORDER BY count DESC
LIMIT 10
```

### 范围查询

```sql
-- 筛选状态码在 400-599 范围内的日志
SELECT *
WHERE status BETWEEN 400 AND 599
```

### 多值比较

```sql
-- 获取多个指标的最大最小值
SELECT
  GREATEST(cpu, memory, disk) as max_usage,
  LEAST(cpu, memory, disk) as min_usage
```
