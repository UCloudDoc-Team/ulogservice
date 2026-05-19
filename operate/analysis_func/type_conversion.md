# 类型转换函数

类型转换函数用于在不同数据类型之间进行转换，支持字符串、数值、时间等类型之间的相互转换。

## 函数列表

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| cast函数 | `cast(x as type)` | 将值转换为指定类型。 |

## 函数说明

### cast

将值转换为指定的数据类型。

**语法**：
```sql
cast(x as type)
```

**支持的数据类型**：
- `bigint`：长整数
- `varchar`：字符串
- `double`：双精度浮点数
- `boolean`：布尔值
- `decimal`：高精度数值

**示例**：
```sql
-- 转换为长整数
SELECT cast('123' as bigint)

-- 转换为字符串
SELECT cast(456 as varchar)

-- 转换为布尔值
SELECT cast('true' as boolean)

-- 转换为双精度浮点数
SELECT cast('3.14' as double)

-- 转换为高精度数值
SELECT cast('123.456' as decimal)
```