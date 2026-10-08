# 检索语法规则

## 操作场景

检索分析语句由检索条件和 SQL 语句组成，两者通过 `|` 分隔，只需要检索日志，不需要统计分析时，可省略其中的 `|` 及 SQL 语句。
```
[检索条件] | [SQL 语句]
```
- **检索条件**：指定日志需要匹配的条件，返回符合该条件的日志。例如使用 `level:error` 检索级别为 error 的日志。检索条件为空或 `*` 时代表无检索条件，即返回所有日志。
> 注意：控制台的检索分析目前无需您手动输入检索条件，添加筛选条件后会自动转换成检索条件语句。<br>在 `Dashboard` 中创建图表，或在 `日志告警` 中设置告警策略时，需要输入完整的检索分析语句。

## 检索语法

> **说明**：检索语法只对创建了索引的字段有效。如需检索某字段，请确保该字段已配置索引。

### 关键字检索

**示例**：
- 检索包含 `error` 的日志：`level:error`
- 检索包含 `exception` 的日志：`level:exception`

### 字段检索

针对特定字段进行检索，不同字段类型支持的查询条件不同。

#### text 类型

| 查询条件 | 含义 | 示例 |
| -------- | ---- | ---- |
| `:` | 包含 | `message:http` |
| `NOT` | 不包含 | `NOT message:http` |
| `=""` | 为空 | `message=""` |
| `!=""` | 不为空 | `message!=""` |

> 注意：`:` 查询不支持通配符（如 `*`、`?`），`message:http` 只匹配包含 `http` 的日志。

#### long 类型

| 查询条件 | 含义 | 示例 |
| -------- | ---- | ---- |
| `=` | 等于 | `id=1` |
| `!=` | 不等于 | `id!=1` |
| `>` | 大于 | `id>1` |
| `>=` | 大于等于 | `id>=1` |
| `<` | 小于 | `id<1` |
| `<=` | 小于等于 | `id<=1` |
| `[a TO b]` | 在指定范围内（含边界） | `id:[1 TO 100]` |
| `{a TO b}` | 在指定范围内（不含边界） | `id:{1 TO 100}` |
| `NOT` | 不在指定范围内 | `NOT id:[1 TO 100]` |

#### double 类型

| 查询条件 | 含义 | 示例 |
| -------- | ---- | ---- |
| `=` | 等于 | `price=1.5` |
| `!=` | 不等于 | `price!=1.5` |
| `>` | 大于 | `price>1.5` |
| `>=` | 大于等于 | `price>=1.5` |
| `<` | 小于 | `price<1.5` |
| `<=` | 小于等于 | `price<=1.5` |
| `[a TO b]` | 在指定范围内（含边界） | `price:[1.0 TO 10.0]` |
| `{a TO b}` | 在指定范围内（不含边界） | `price:{1.0 TO 10.0}` |
| `NOT` | 不在指定范围内 | `NOT price:[1.0 TO 10.0]` |

### 组合检索

支持使用 `AND`、`OR`、`NOT` 运算符组合多个检索条件，并可使用 `()` 对条件进行分组。`AND` 表示同时满足多个条件，`OR` 表示满足其中任意一个条件，`NOT` 表示排除满足该条件的日志。

**示例**：
- 检索 level 包含 ERROR 且 status 大于 400 的日志：`level:ERROR AND status>400`
- 检索 method 包含 GET 且 response_time 大于 1000 的日志：`method:GET AND response_time>1000`
- 检索 level 包含 ERROR 或 WARN 的日志：`level:ERROR OR level:WARN`
- 检索 level 包含 ERROR 或 WARN，且 status 大于 400 的日志：`(level:ERROR OR level:WARN) AND status>400`
- 检索 level 包含 ERROR 且 method 不包含 GET 的日志：`level:ERROR AND NOT method:GET`

---

## 使用示例

1. **检索 message 字段包含 http 的日志**
   ```
   message:http
   ```

2. **检索 message 字段不包含 http 的日志**
   ```
   NOT message:http
   ```

3. **检索 message 字段为空的日志**
   ```
   message=""
   ```

4. **检索 message 字段不为空的日志**
   ```
   message!=""
   ```

5. **检索状态码为 404 的日志**
   ```
   status=404
   ```

6. **检索响应时间超过 1000ms 的请求**
   ```
   response_time>1000
   ```

7. **检索 ID 在 1 到 100 范围内（含 1 和 100）的日志**
   ```
   id:[1 TO 100]
   ```

8. **检索 ID 在 1 到 100 范围内（不含 1 和 100）的日志**
   ```
   id:{1 TO 100}
   ```

9. **检索 ID 不在 1 到 100 范围内的日志**
   ```
   NOT id:[1 TO 100]
   ```

10. **检索价格在 1.0 到 10.0 范围内的日志**
    ```
    price:[1.0 TO 10.0]
    ```

11. **组合检索：检索 level 包含 ERROR 且 status 大于 400 的日志**
    ```
    level:ERROR AND status>400
    ```

12. **组合检索：检索 level 包含 ERROR 或 WARN 的日志**
    ```
    level:ERROR OR level:WARN
    ```

13. **组合检索：检索 level 包含 ERROR 或 WARN，且 status 大于 400 的日志**
    ```
    (level:ERROR OR level:WARN) AND status>400
    ```

14. **检索并分析：检索 level 包含 ERROR 且 status 大于 400 的日志，并按状态码分组统计**
    ```
    level:ERROR AND status>400 | SELECT status, COUNT(*) as count GROUP BY status
    ```
