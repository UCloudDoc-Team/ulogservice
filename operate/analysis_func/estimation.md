# 估算函数

估算函数用于对大数据集进行近似计算，在保证一定精度的情况下提高计算效率。

## 函数列表

| 名称 | 语法 | 说明 |
|------|------|------|
| approx_distinct函数 | `approx_distinct(x)` | 估算 x 中不重复值的个数，默认存在 2.3% 的标准误差。 |
| approx_percentile函数 | `approx_percentile(x, percentage)` | 对 x 进行正序排列，返回大约处于 percentage 位置的 x。最终的结果是近似结果，不保证稳定性和一致性。 |
| | `approx_percentile(x, [percentage01, percentage02...])` | 对 x 进行正序排列，返回大约处于 percentage01、percentage02 位置的 x。最终的结果是近似结果，不保证稳定性和一致性。 |
| approx_most_frequent函数 | `approx_most_frequent(k, x)` | 估算 x 列最常出现的 k 个值以及每个值出现的近似次数，返回 MAP 类型。 |

## 函数说明

### approx_distinct

估算 x 中不重复值的个数，默认存在 2.3% 的标准误差。

**语法**：
```sql
approx_distinct(x)
```

**示例**：
```sql
-- 估算独立访问 IP 数量
SELECT approx_distinct(client_ip) as unique_ips

-- 按状态码估算独立用户数
SELECT
  status,
  approx_distinct(user_id) as unique_users
GROUP BY status
```

### approx_percentile

对 x 进行正序排列，返回大约处于指定百分比位置的值。支持单个百分位和多个百分位查询。

**语法**：
```sql
approx_percentile(x, percentage)
approx_percentile(x, [percentage01, percentage02...])
```

**参数说明**：
- x：数值类型字段
- percentage：百分位，取值范围 0 到 1，如 0.5 表示中位数

**示例**：
```sql
-- 估算响应时间的 P50
SELECT approx_percentile(response_time, 0.5) as p50

-- 估算响应时间的 P50、P90、P99
SELECT approx_percentile(response_time, [0.5, 0.9, 0.99]) as percentiles

-- 按请求方法计算 P95 响应时间
SELECT
  method,
  approx_percentile(response_time, 0.95) as p95_time
GROUP BY method
```

### approx_most_frequent

估算 x 列最常出现的 k 个值以及每个值出现的近似次数，返回 MAP 类型。

**语法**：
```sql
approx_most_frequent(k, x)
```

**参数说明**：
- k：返回最常出现的值个数
- x：要统计的字段

**示例**：
```sql
-- 获取访问量前 10 的 IP 地址及其出现次数
SELECT approx_most_frequent(10, client_ip) as top_ips

-- 获取最常见的 5 个请求路径及其出现次数
SELECT approx_most_frequent(5, path) as top_paths
```

## 使用示例

### 性能分析

```sql
-- 分析响应时间分布
SELECT
  count(*) as total,
  avg(response_time) as avg_time,
  approx_percentile(response_time, 0.5) as p50,
  approx_percentile(response_time, 0.9) as p90,
  approx_percentile(response_time, 0.99) as p99
```

### 用户访问分析

```sql
-- 分析各状态码的独立访客数
SELECT
  status,
  count(*) as requests,
  approx_distinct(client_ip) as unique_ips,
  approx_distinct(user_id) as unique_users
GROUP BY status
ORDER BY requests DESC
```

### 热点资源分析

```sql
-- 分析访问量最高的资源
SELECT
  approx_most_frequent(10, path) as top_paths
WHERE status = 200
```

## 注意事项

1. 估算函数返回的是近似值，适用于大数据量场景
2. approx_distinct 默认存在 2.3% 的标准误差
3. approx_percentile 的百分位参数范围为 0-1
4. approx_percentile 的结果是近似结果，不保证稳定性和一致性
5. approx_most_frequent 返回结果为 MAP 类型，包含值和对应的出现次数
