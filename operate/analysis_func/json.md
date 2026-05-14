# JSON 函数

JSON 函数用于解析和处理 JSON 格式的数据。

## 函数列表

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| json_array_length函数 | `json_array_length(x)` | 计算 JSON 数组中元素的数量。 |
| json_parse函数 | `json_parse(x)` | 把字符串类型转换为 JSON 类型。 |

## 函数说明

### json_array_length

计算 JSON 数组中元素的数量。

**语法**：
```sql
json_array_length(x)
```

**示例**：
```sql
-- 获取数组长度
SELECT json_array_length('[1, 2, 3, 4, 5]')
-- 返回: 5

-- 获取空数组长度
SELECT json_array_length('[]')
-- 返回: 0

-- 统计日志中数组字段的元素数量
SELECT json_array_length(items) as items_count
```

### json_parse

把字符串类型转换为 JSON 类型。

**语法**：
```sql
json_parse(x)
```

**示例**：
```sql
-- 解析 JSON 字符串
SELECT json_parse('{"name": "张三", "age": 25}')

-- 解析数组字符串
SELECT json_parse('[1, 2, 3]')

-- 解析日志中的 JSON 字符串
SELECT json_parse(log_data)
```

## 使用示例

### 解析 JSON 字符串

```sql
-- 将字符串字段转换为 JSON 类型
SELECT
  json_parse(request_body) as request_json
WHERE request_body IS NOT NULL
```

### 统计数组元素数量

```sql
-- 统计各日志中数组字段的元素数量分布
SELECT
  json_array_length(items) as array_size,
  count(*) as count
WHERE items IS NOT NULL
GROUP BY array_size
ORDER BY array_size
```

### 结合使用

```sql
-- 先解析字符串为 JSON，再统计数组长度
SELECT
  json_array_length(json_parse(items_str)) as items_count
WHERE items_str IS NOT NULL
```
