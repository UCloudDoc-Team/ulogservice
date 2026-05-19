# 分析语法规则

## 操作场景

日志分析支持类 SQL 语法，可用于对日志数据进行统计分析。比如针对符合检索条件的日志进行统计分析，返回统计分析结果。例如使用 `SELECT COUNT(*) as cnt WHERE level='error'` 统计级别为 error 的日志数量。

## 操作步骤
1. 登录日志服务控制台。
2. 在顶部的栏目中选择检索分析。
3. 在右侧选择统计图表。
4. 在统计图表中写入要执行的SQL分析语句。

## 基本结构

```sql
SELECT 字段列表 | 聚合函数
FROM 日志数据
WHERE 筛选条件
GROUP BY 分组字段
HAVING 分组后筛选条件
ORDER BY 排序字段 [ASC|DESC]
LIMIT 限制条数
```

## 分组统计

使用 `GROUP BY` 对结果进行分组统计。

**示例**：
- 按 `status` 字段分组统计日志条数：
  ```sql
  SELECT status, COUNT(*) as count
  GROUP BY status
  ```

- 按 `method` 和 `status` 分组统计：
  ```sql
  SELECT method, status, COUNT(*) as count
  GROUP BY method, status
  ```

## 排序

使用 `ORDER BY` 对结果进行排序，支持 `ASC`（升序）和 `DESC`（降序）。

**示例**：
- 按日志条数降序排列：
  ```sql
  SELECT status, COUNT(*) as count
  GROUP BY status
  ORDER BY count DESC
  ```

## 限制结果条数

使用 `LIMIT` 限制返回的结果条数。

**示例**：
- 返回前 10 条结果：
  ```sql
  SELECT *
  LIMIT 10
  ```
不填写LIMIT默认值是10。

## 条件筛选

使用 `WHERE` 子句添加筛选条件。

**示例**：
- 筛选状态码大于 400 的日志并按状态码分组统计：
  ```sql
  SELECT status, COUNT(*) as count
  WHERE status > 400
  GROUP BY status
  ```

## 分组后筛选

使用 `HAVING` 子句对分组后的结果进行筛选，通常与 `GROUP BY` 配合使用，支持对聚合结果进行过滤。

**示例**：
- 筛选日志条数大于 100 的状态码：
  ```sql
  SELECT status, COUNT(*) as count
  GROUP BY status
  HAVING count > 100
  ```

- 筛选平均响应时间超过 500ms 的请求方法：
  ```sql
  SELECT method, AVG(response_time) as avg_time
  GROUP BY method
  HAVING avg_time > 500
  ```

---

## 使用示例

1. **统计各状态码的日志数量**
   ```sql
   SELECT status, COUNT(*) as count
   GROUP BY status
   ORDER BY count DESC
   ```

2. **统计各请求方法的平均响应时间**
   ```sql
   SELECT method, AVG(response_time) as avg_time
   GROUP BY method
   ```

3. **统计访问量前 10 的 IP 地址**
   ```sql
   SELECT client_ip, COUNT(*) as count
   GROUP BY client_ip
   ORDER BY count DESC
   LIMIT 10
   ```