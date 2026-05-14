# 字符串函数

字符串函数用于处理和操作文本数据，支持字符串截取、拼接、查找、替换等操作。

## 函数列表

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| chr函数 | `chr(x)` | 将 ASCII 码转换为字符。 |
| codepoint函数 | `codepoint(x)` | 将字符转换为 ASCII 码。 |
| concat函数 | `concat(x, y...)` | 将多个字符串拼接成一个字符串。 |
| from_utf8函数 | `from_utf8(x)` | 将二进制字符串解码为 UTF-8 编码格式，并使用默认字符 U+FFFD 替换无效的 UTF-8 字符。 |
| length函数 | `length(x)` | 计算字符串的长度。 |
| lower函数 | `lower(x)` | 将字符串转换为小写形式。 |
| lpad函数 | `lpad(x, length, lpad_string)` | 在字符串的开头填充指定字符，直到指定长度后返回结果字符串。 |
| ltrim函数 | `ltrim(x)` | 删除字符串开头的空格。 |
| position函数 | `position(sub_string in x)` | 返回目标子串在字符串中的位置。 |
| replace函数 | `replace(x, sub_string)` | 删除字符串中匹配的字符。 |
| | `replace(x, sub_string, replace_string)` | 将字符串中所匹配的字符替换为其他指定字符。 |
| reverse函数 | `reverse(x)` | 返回反向顺序的字符串。 |
| rpad函数 | `rpad(x, length, rpad_string)` | 在字符串的尾部填充指定字符，直到指定长度后返回结果字符串。 |
| rtrim函数 | `rtrim(x)` | 删除字符串中结尾的空格。 |
| split函数 | `split(x, delimeter)` | 使用指定的分隔符拆分字符串，并返回子串集合。 |
| split_part函数 | `split_part(x, delimeter, part)` | 使用指定的分隔符拆分字符串，并返回指定位置的内容。 |
| split_to_map函数 | `split_to_map(x, delimiter01, delimiter02)` | 使用指定的第一个分隔符拆分字符串，然后再使用指定的第二个分隔符进行第二次拆分。 |
| strpos函数 | `strpos(x, sub_string)` | 返回目标子串在字符串中的位置。与 position(sub_string in x) 函数等价。 |
| substr函数 | `substr(x, start)` | 返回字符串中指定位置的子串。 |
| | `substr(x, start, length)` | 返回字符串中指定位置的子串，并指定子串长度。 |
| to_utf8函数 | `to_utf8(x)` | 将字符串转换为 UTF-8 编码格式。 |
| trim函数 | `trim(x)` | 删除字符串中开头和结尾的空格。 |
| upper函数 | `upper(x)` | 将字符串转化为大写形式。 |
| str_uuid函数 | `str_uuid()` | 生成一个随机的 128 位 ID，并以字符串 (String) 格式返回。 |

## 函数说明

### chr

将 ASCII 码转换为字符。

**语法**：
```sql
chr(x)
```

**示例**：
```sql
-- 将 ASCII 码 65 转换为字符
SELECT chr(65) as char_val
-- 返回: A
```

### codepoint

将字符转换为 ASCII 码。

**语法**：
```sql
codepoint(x)
```

**示例**：
```sql
-- 将字符 A 转换为 ASCII 码
SELECT codepoint('A') as ascii_val
-- 返回: 65
```

### concat

将多个字符串拼接成一个字符串。

**语法**：
```sql
concat(x, y...)
```

**示例**：
```sql
-- 拼接请求方法和路径
SELECT concat(method, ' ', path) as request

-- 拼接多个字段
SELECT concat(client_ip, ':', port) as client_address
```

### from_utf8

将二进制字符串解码为 UTF-8 编码格式，并使用默认字符 U+FFFD 替换无效的 UTF-8 字符。

**语法**：
```sql
from_utf8(x)
```

**示例**：
```sql
-- 将二进制字符串解码为 UTF-8
SELECT from_utf8(binary_data) as decoded_str
```

### length

计算字符串的长度。

**语法**：
```sql
length(x)
```

**示例**：
```sql
-- 获取消息长度
SELECT message, length(message) as msg_length

-- 筛选长消息
SELECT *
WHERE length(message) > 100
```

### lower / upper

转换字符串大小写。

**语法**：
```sql
lower(x)  -- 转换为小写
upper(x)  -- 转换为大写
```

**示例**：
```sql
-- 转换为小写
SELECT lower(method) as method_lower

-- 转换为大写
SELECT upper(status) as status_upper

-- 不区分大小写匹配
SELECT *
WHERE lower(path) LIKE '%api%'
```

### lpad / rpad

在字符串的开头或尾部填充指定字符。

**语法**：
```sql
lpad(x, length, lpad_string)  -- 在开头填充
rpad(x, length, rpad_string)  -- 在尾部填充
```

**示例**：
```sql
-- 在开头填充 0，使字符串长度为 10
SELECT lpad(id, 10, '0') as padded_id

-- 在尾部填充空格，使字符串长度为 20
SELECT rpad(name, 20, ' ') as padded_name
```

### trim / ltrim / rtrim

删除字符串中的空格。

**语法**：
```sql
trim(x)    -- 删除开头和结尾的空格
ltrim(x)   -- 删除开头的空格
rtrim(x)   -- 删除结尾的空格
```

**示例**：
```sql
-- 去除首尾空格
SELECT trim(user_input) as cleaned_input

-- 去除开头空格
SELECT ltrim(message) as msg

-- 去除结尾空格
SELECT rtrim(content) as trimmed_content
```

### position / strpos

返回目标子串在字符串中的位置。

**语法**：
```sql
position(sub_string in x)  -- 返回子串位置
strpos(x, sub_string)      -- 与 position 函数等价
```

**示例**：
```sql
-- 查找关键词位置
SELECT strpos(message, 'error') as error_pos

-- 使用 position 语法
SELECT position('error' in message) as error_pos

-- 筛选包含特定关键词的日志
SELECT *
WHERE strpos(message, 'error') > 0
```

### replace

删除或替换字符串中的指定内容。

**语法**：
```sql
replace(x, sub_string)                      -- 删除匹配的字符
replace(x, sub_string, replace_string)      -- 替换匹配的字符
```

**示例**：
```sql
-- 删除字符串中的特定字符
SELECT replace(url, 'http://') as clean_url

-- 替换敏感信息
SELECT replace(url, 'password=xxx', 'password=***') as safe_url
```

### reverse

返回反向顺序的字符串。

**语法**：
```sql
reverse(x)
```

**示例**：
```sql
-- 反转字符串
SELECT reverse(path) as reversed_path
```

### split

使用指定的分隔符拆分字符串，并返回子串集合。

**语法**：
```sql
split(x, delimiter)
```

**示例**：
```sql
-- 按逗号分割字符串
SELECT split(tags, ',') as tag_list

-- 按斜杠分割路径
SELECT split(path, '/') as path_parts
```

### split_part

使用指定的分隔符拆分字符串，并返回指定位置的内容。

**语法**：
```sql
split_part(x, delimiter, part)
```

**示例**：
```sql
-- 提取路径的第一部分
SELECT split_part(path, '/', 1) as first_segment

-- 提取 URL 中的域名
SELECT split_part(url, '/', 2) as domain
```

### split_to_map

使用指定的分隔符进行两次拆分，返回 MAP 类型。

**语法**：
```sql
split_to_map(x, delimiter01, delimiter02)
```

**示例**：
```sql
-- 解析查询字符串为 MAP
SELECT split_to_map('a=1&b=2&c=3', '&', '=') as query_map
-- 返回: {a: 1, b: 2, c: 3}
```

### substr

返回字符串中指定位置的子串。

**语法**：
```sql
substr(x, start)           -- 从指定位置截取到末尾
substr(x, start, length)   -- 从指定位置截取指定长度
```

**示例**：
```sql
-- 截取前 10 个字符
SELECT substr(message, 1, 10) as preview

-- 从第 5 个字符截取到末尾
SELECT substr(path, 5) as sub_path
```

### to_utf8

将字符串转换为 UTF-8 编码格式。

**语法**：
```sql
to_utf8(x)
```

**示例**：
```sql
-- 将字符串转换为 UTF-8 编码
SELECT to_utf8(content) as utf8_bytes
```

### str_uuid

生成一个随机的 128 位 ID，并以字符串格式返回。

**语法**：
```sql
str_uuid()
```

**示例**：
```sql
-- 生成随机 UUID
SELECT str_uuid()
```

## 使用示例

### URL 路径分析

```sql
-- 提取和分析 URL 路径
SELECT
  path,
  split_part(path, '/', 1) as first_part,
  length(path) as path_length
GROUP BY path
ORDER BY count(*) DESC
LIMIT 10
```

### 日志清洗

```sql
-- 清洗日志消息
SELECT
  trim(message) as clean_message,
  upper(method) as method_upper,
  replace(message, '\n', ' ') as single_line
```

### 字符串匹配统计

```sql
-- 统计包含特定关键词的日志
SELECT
  method,
  count(*) as count
WHERE strpos(path, '/api/') > 0
GROUP BY method
ORDER BY count DESC
```
