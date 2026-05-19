# 日期和时间函数

日期和时间函数用于处理时间类型数据，支持时间格式化、解析、计算等操作。

## 函数列表

### 日期和时间函数

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| current_date函数 | `current_date` | 返回当前日期。 |
| current_time函数 | `current_time` | 返回当前时间。 |
| current_timestamp函数 | `current_timestamp` | 返回当前时间戳。 |
| date_format函数 | `date_format(x, format)` | 格式化时间。 |
| date_parse函数 | `date_parse(x, format)` | 解析时间字符串。 |
| from_unixtime函数 | `from_unixtime(x)` | 将 Unix 时间戳转换为时间字符串。 |
| localtime函数 | `localtime` | 返回本地时间。 |
| localtimestamp函数 | `localtimestamp` | 返回本地时间戳。 |
| now函数 | `now()` | 返回当前时间。 |
| to_iso8601函数 | `to_iso8601(x)` | 将时间转换为 ISO8601 格式字符串。 |
| to_unixtime函数 | `to_unixtime(x)` | 将时间转换为 Unix 时间戳。 |
| current_unixtimestamp函数 | `current_unixtimestamp()` | 返回当前 Unix 时间戳。 |

### 日期和时间提取函数

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| day函数 | `day(x)` | 返回日期中的日（1-31）。 |
| day_of_month函数 | `day_of_month(x)` | 返回日期中的日（1-31），与 day 函数等价。 |
| day_of_week函数 | `day_of_week(x)` | 返回星期几（1-7，周日为 1）。 |
| day_of_year函数 | `day_of_year(x)` | 返回一年中的第几天（1-366）。 |
| dow函数 | `dow(x)` | 返回星期几，与 day_of_week 函数等价。 |
| doy函数 | `doy(x)` | 返回一年中的第几天，与 day_of_year 函数等价。 |
| hour函数 | `hour(x)` | 返回小时（0-23）。 |
| minute函数 | `minute(x)` | 返回分钟（0-59）。 |
| month函数 | `month(x)` | 返回月份（1-12）。 |
| quarter函数 | `quarter(x)` | 返回季度（1-4）。 |
| second函数 | `second(x)` | 返回秒数（0-59）。 |
| week函数 | `week(x)` | 返回一年中的第几周（1-53）。 |
| week_of_year函数 | `week_of_year(x)` | 返回一年中的第几周，与 week 函数等价。 |
| year函数 | `year(x)` | 返回年份。 |

### 时间间隔函数

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| date_trunc函数 | `date_trunc(unit, x)` | 按指定精度截断时间。 |
| date_add函数 | `date_add(unit, N, x)` | 在时间上增加指定的时间间隔。 |
| date_diff函数 | `date_diff(unit, x, y)` | 计算两个时间之间的差值。 |

## 函数说明

### 日期和时间函数

#### current_date / current_time / current_timestamp

返回当前日期、时间或时间戳。

**语法**：
```sql
current_date        -- 当前日期
current_time        -- 当前时间
current_timestamp   -- 当前时间戳
```

**示例**：
```sql
-- 获取当前日期
SELECT current_date

-- 获取当前时间
SELECT current_time

-- 获取当前时间戳
SELECT current_timestamp
```

#### date_format

将时间格式化为指定格式的字符串。

**语法**：
```sql
date_format(x, format)
```

**格式说明**：
| 格式符 | 说明 |
|--------|------|
| %Y | 四位年份 |
| %m | 月份 (01-12) |
| %d | 日期 (01-31) |
| %H | 小时 (00-23) |
| %i | 分钟 (00-59) |
| %s | 秒 (00-59) |

**示例**：
```sql
-- 格式化为年-月-日
SELECT date_format(__time__, '%Y-%m-%d')

-- 格式化为完整时间
SELECT date_format(__time__, '%Y-%m-%d %H:%i:%s')
```

#### date_parse

将字符串解析为时间类型。

**语法**：
```sql
date_parse(x, format)
```

**示例**：
```sql
-- 解析标准日期格式
SELECT date_parse('2024-01-15', '%Y-%m-%d')

-- 解析自定义格式
SELECT date_parse('15/01/2024', '%d/%m/%Y')
```

#### from_unixtime

将 Unix 时间戳转换为时间字符串。

**语法**：
```sql
from_unixtime(x)
```

**示例**：
```sql
-- 将 Unix 时间戳转换为时间字符串
SELECT from_unixtime(1705286400)
```

#### localtime / localtimestamp

返回本地时间和本地时间戳。

**语法**：
```sql
localtime        -- 本地时间
localtimestamp   -- 本地时间戳
```

**示例**：
```sql
SELECT localtime
SELECT localtimestamp
```

#### now

返回当前时间。

**语法**：
```sql
now()
```

**示例**：
```sql
-- 获取当前时间
SELECT now()
```

#### to_iso8601

将时间转换为 ISO8601 格式字符串。

**语法**：
```sql
to_iso8601(x)
```

**示例**：
```sql
-- 转换为 ISO8601 格式
SELECT to_iso8601(__time__)
```

#### to_unixtime

将时间转换为 Unix 时间戳。

**语法**：
```sql
to_unixtime(x)
```

**示例**：
```sql
-- 转换为 Unix 时间戳
SELECT to_unixtime(__time__)
```

#### current_unixtimestamp

返回当前 Unix 时间戳。

**语法**：
```sql
current_unixtimestamp()
```

**示例**：
```sql
-- 获取当前 Unix 时间戳
SELECT current_unixtimestamp()
```

### 日期和时间提取函数

#### year / month / day

提取年、月、日部分。

**语法**：
```sql
year(x)    -- 年份
month(x)   -- 月份（1-12）
day(x)     -- 日期（1-31）
```

**示例**：
```sql
-- 提取各时间部分
SELECT
  year(__time__) as year,
  month(__time__) as month,
  day(__time__) as day

-- 按月统计
SELECT
  year(__time__) as year,
  month(__time__) as month,
  count(*) as count
GROUP BY year, month
ORDER BY year, month
```

#### hour / minute / second

提取时、分、秒部分。

**语法**：
```sql
hour(x)     -- 小时（0-23）
minute(x)   -- 分钟（0-59）
second(x)   -- 秒数（0-59）
```

**示例**：
```sql
-- 按小时统计请求量
SELECT
  hour(__time__) as hour,
  count(*) as count
GROUP BY hour
ORDER BY hour
```

#### day_of_week / dow

返回星期几（1-7，周日为 1）。

**语法**：
```sql
day_of_week(x)
dow(x)
```

**示例**：
```sql
-- 按星期统计请求量
SELECT
  day_of_week(__time__) as weekday,
  count(*) as count
GROUP BY weekday
ORDER BY weekday
```

#### day_of_year / doy

返回一年中的第几天（1-366）。

**语法**：
```sql
day_of_year(x)
doy(x)
```

**示例**：
```sql
SELECT day_of_year(__time__)
```

#### week / week_of_year

返回一年中的第几周（1-53）。

**语法**：
```sql
week(x)
week_of_year(x)
```

**示例**：
```sql
-- 按周统计
SELECT
  week(__time__) as week_num,
  count(*) as count
GROUP BY week_num
ORDER BY week_num
```

#### quarter

返回季度（1-4）。

**语法**：
```sql
quarter(x)
```

**示例**：
```sql
-- 按季度统计
SELECT
  quarter(__time__) as q,
  count(*) as count
GROUP BY q
ORDER BY q
```

### 时间间隔函数

#### date_trunc

按指定精度截断时间。

**支持的单位**：`second`、`minute`、`hour`、`day`、`week`、`month`、`quarter`、`year`。

**语法**：
```sql
date_trunc(unit, x)
```

**示例**：
```sql
-- 按小时截断
SELECT date_trunc('hour', __time__), count(*)
GROUP BY date_trunc('hour', __time__)

-- 按天截断
SELECT date_trunc('day', __time__), count(*)
GROUP BY date_trunc('day', __time__)

-- 按月截断
SELECT date_trunc('month', __time__), count(*)
GROUP BY date_trunc('month', __time__)
```

#### date_add

在时间上增加指定的时间间隔。

**语法**：
```sql
date_add(unit, N, x)
```

**支持的单位**：`second`、`minute`、`hour`、`day`、`week`、`month`、`quarter`、`year`。

**示例**：
```sql
-- 增加 1 天
SELECT date_add('day', 1, __time__)

-- 增加 2 小时
SELECT date_add('hour', 2, __time__)

-- 减少 1 周（N 为负数）
SELECT date_add('week', -1, __time__)
```

#### date_diff

计算两个时间之间的差值。

**语法**：
```sql
date_diff(unit, x, y)
```

**支持的单位**：`second`、`minute`、`hour`、`day`、`week`、`month`、`quarter`、`year`。

**示例**：
```sql
-- 计算相差的天数
SELECT date_diff('day', __time__, now())

-- 计算相差的小时数
SELECT date_diff('hour', start_time, end_time)
```

## 使用示例

### 时间分布分析

```sql
-- 分析每小时的请求分布
SELECT
  hour(__time__) as hour,
  count(*) as requests,
  avg(response_time) as avg_time
GROUP BY hour
ORDER BY hour
```

### 时间格式化

```sql
-- 格式化时间显示
SELECT
  date_format(__time__, '%Y-%m-%d %H:%i:%s') as formatted_time,
  count(*) as count
GROUP BY date_format(__time__, '%Y-%m-%d %H:%i:%s')
ORDER BY formatted_time
```

### 时间范围计算

```sql
-- 计算时间差
SELECT
  date_diff('hour', __time__, now()) as hours_ago,
  count(*) as count
GROUP BY hours_ago
ORDER BY hours_ago
```
