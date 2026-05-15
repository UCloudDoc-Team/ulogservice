# 数学计算函数

数学计算函数用于执行各种数学运算，包括基本算术运算、三角函数、取整函数等。

## 函数列表

| 函数名称 | 语法 | 说明 |
|----------|------|------|
| abs函数 | `abs(x)` | 计算 x 的绝对值。 |
| acos函数 | `acos(x)` | 计算 x 的反余弦。 |
| asin函数 | `asin(x)` | 计算 x 的反正弦。 |
| atan函数 | `atan(x)` | 计算 x 的反正切。 |
| atan2函数 | `atan2(x, y)` | 计算 x 和 y 相除的结果的反正切。 |
| cbrt函数 | `cbrt(x)` | 计算 x 的立方根。 |
| ceil函数 | `ceil(x)` | 对 x 进行向上取整数。ceil 函数是 ceiling 函数的别名。 |
| ceiling函数 | `ceiling(x)` | 对 x 进行向上取整数。 |
| cos函数 | `cos(x)` | 计算 x 的余弦。 |
| cosh函数 | `cosh(x)` | 计算 x 的双曲余弦。 |
| degrees函数 | `degrees(x)` | 将弧度转换为度。 |
| e函数 | `e()` | 返回自然底数 e 的值。 |
| exp函数 | `exp(x)` | 计算自然底数 e 的 x 次幂。 |
| floor函数 | `floor(x)` | 对 x 进行向下取整数。 |
| ln函数 | `ln(x)` | 计算 x 的自然对数。 |
| log2函数 | `log2(x)` | 计算 x 以 2 为底的对数。 |
| log10函数 | `log10(x)` | 计算 x 以 10 为底的对数。 |
| log函数 | `log(x, y)` | 计算 x 以 y 为底的对数。 |
| mod函数 | `mod(x, y)` | 计算 x 与 y 相除的余数。 |
| pi函数 | `pi()` | 返回 π 值，精确到小数点后 15 位。 |
| pow函数 | `pow(x, y)` | 计算 x 的 y 次幂。pow 函数是 power 函数的别名。 |
| power函数 | `power(x, y)` | 计算 x 的 y 次幂。 |
| radians函数 | `radians(x)` | 将度转换为弧度。 |
| rand函数 | `rand()` | 返回随机数。 |
| random函数 | `random()` | 返回 [0,1) 之间的随机数。 |
| round函数 | `round(x)` | 对 x 进行四舍五入取整数。 |
| | `round(x, n)` | 对 x 进行四舍五入且保留 n 位小数。 |
| sign函数 | `sign(x)` | 返回 x 的符号，通过 1、0、-1 表示。 |
| sin函数 | `sin(x)` | 计算 x 的正弦。 |
| sqrt函数 | `sqrt(x)` | 计算 x 的平方根。 |
| tan函数 | `tan(x)` | 计算 x 的正切。 |
| tanh函数 | `tanh(x)` | 计算 x 的双曲正切。 |
| truncate函数 | `truncate(x)` | 截断 x 的小数部分。 |

## 函数说明

### abs

计算 x 的绝对值。

**语法**：
```sql
abs(x)
```

**示例**：
```sql
SELECT abs(-123)
-- 返回: 123
```

### ceil / ceiling

对 x 进行向上取整数。ceil 函数是 ceiling 函数的别名。

**语法**：
```sql
ceil(x)
ceiling(x)
```

**示例**：
```sql
SELECT ceil(3.2)
-- 返回: 4

SELECT ceiling(-3.2)
-- 返回: -3
```

### floor

对 x 进行向下取整数。

**语法**：
```sql
floor(x)
```

**示例**：
```sql
SELECT floor(3.8)
-- 返回: 3

SELECT floor(-3.8)
-- 返回: -4
```

### round

对 x 进行四舍五入取整数，或保留指定小数位数。

**语法**：
```sql
round(x)      -- 四舍五入取整数
round(x, n)   -- 四舍五入且保留 n 位小数
```

**示例**：
```sql
SELECT round(3.14159)
-- 返回: 3

SELECT round(3.14159, 2)
-- 返回: 3.14

SELECT round(3.5)
-- 返回: 4
```

### truncate

截断 x 的小数部分。

**语法**：
```sql
truncate(x)
```

**示例**：
```sql
SELECT truncate(3.14159)
-- 返回: 3
```

### mod

计算 x 与 y 相除的余数。

**语法**：
```sql
mod(x, y)
```

**示例**：
```sql
SELECT mod(10, 3)
-- 返回: 1
```

### power / pow

计算 x 的 y 次幂。pow 函数是 power 函数的别名。

**语法**：
```sql
power(x, y)
pow(x, y)
```

**示例**：
```sql
SELECT power(2, 10)
-- 返回: 1024

SELECT pow(3, 2)
-- 返回: 9
```

### sqrt

计算 x 的平方根。

**语法**：
```sql
sqrt(x)
```

**示例**：
```sql
SELECT sqrt(16)
-- 返回: 4
```

### cbrt

计算 x 的立方根。

**语法**：
```sql
cbrt(x)
```

**示例**：
```sql
SELECT cbrt(27)
-- 返回: 3
```

### exp

计算自然底数 e 的 x 次幂。

**语法**：
```sql
exp(x)
```

**示例**：
```sql
SELECT exp(1)
-- 返回: 约 2.718
```

### ln

计算 x 的自然对数。

**语法**：
```sql
ln(x)
```

**示例**：
```sql
SELECT ln(2.71828)
-- 返回: 约 1
```

### log

计算 x 以 y 为底的对数。

**语法**：
```sql
log(x, y)
```

**示例**：
```sql
SELECT log(8, 2)
-- 返回: 3
```

### log2

计算 x 以 2 为底的对数。

**语法**：
```sql
log2(x)
```

**示例**：
```sql
SELECT log2(8)
-- 返回: 3
```

### log10

计算 x 以 10 为底的对数。

**语法**：
```sql
log10(x)
```

**示例**：
```sql
SELECT log10(100)
-- 返回: 2
```

### 三角函数

**语法**：
```sql
sin(x)     -- 计算 x 的正弦
cos(x)     -- 计算 x 的余弦
tan(x)     -- 计算 x 的正切
asin(x)    -- 计算 x 的反正弦
acos(x)    -- 计算 x 的反余弦
atan(x)    -- 计算 x 的反正切
atan2(x, y) -- 计算 x 和 y 相除的结果的反正切
sinh(x)    -- 计算 x 的双曲正弦
cosh(x)    -- 计算 x 的双曲余弦
tanh(x)    -- 计算 x 的双曲正切
```

**示例**：
```sql
SELECT sin(radians(30))
-- 返回: 约 0.5

SELECT cos(0)
-- 返回: 1

SELECT degrees(acos(0.5))
-- 返回: 60
```

### degrees / radians

角度和弧度转换。

**语法**：
```sql
degrees(x)   -- 将弧度转换为度
radians(x)   -- 将度转换为弧度
```

**示例**：
```sql
SELECT degrees(3.14159)
-- 返回: 约 180

SELECT radians(180)
-- 返回: 约 3.14159
```

### sign

返回 x 的符号，通过 1、0、-1 表示。

**语法**：
```sql
sign(x)
```

**返回值**：
- 正数返回 1
- 零返回 0
- 负数返回 -1

**示例**：
```sql
SELECT sign(-100)
-- 返回: -1

SELECT sign(100)
-- 返回: 1

SELECT sign(0)
-- 返回: 0
```

### pi / e

返回常量值。

**语法**：
```sql
pi()   -- 返回 π 值，精确到小数点后 15 位
e()    -- 返回自然底数 e 的值
```

**示例**：
```sql
SELECT pi()
-- 返回: 3.141592653589793

SELECT e()
-- 返回: 2.718281828459045
```

### rand / random

返回随机数。

**语法**：
```sql
rand()     -- 返回随机数
random()   -- 返回 [0,1) 之间的随机数
```

**示例**：
```sql
SELECT rand()
SELECT random()
```
