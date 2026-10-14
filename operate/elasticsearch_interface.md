# 兼容 Elasticsearch 接口

## 操作场景

日志服务（ULogService）兼容 Elasticsearch 的部分接口，其兼容能力基于 Elasticsearch 7.10 实现，支持 **7.x 及以下版本**（客户端版本 ≤ 7.x）的 Elasticsearch 客户端接入。如果您已有的日志采集、检索或可视化程序是基于 Elasticsearch 构建的，无需改造代码，只需将请求地址指向日志服务的兼容接口，即可直接查询日志服务中已上报的日志，从而降低日志系统迁移的成本。

## 前提条件

使用兼容接口前，请确认：

1. 日志主题（Topic）已创建，且日志已正常上报，详情请参阅 [日志主题](/ulogservice/resource/topic)。
2. **需要查询的字段已开启索引**。未开启索引的字段无法通过兼容接口检索，索引配置方法请参阅 [日志主题 > 修改索引配置](/ulogservice/resource/topic)。
3. 使用聚合或排序时，对应字段需在索引配置中额外开启统计。

> **说明**：
> - 使用 `term`、`terms`、`match_phrase`、`range` 等查询 DSL 时，目标字段必须已开启索引。
> - 使用聚合（桶聚合、指标聚合）或通过 `sort` 排序时，目标字段必须已开启索引，且已开启统计。

## 访问地址

日志服务的兼容接口通过各 [地域](/ulogservice/region_domain_name) 的内网域名提供，请求路径统一以 `/elasticsearch/` 开头：

```
https://internal.<region>.uls.ucloud.cn/elasticsearch/
```

其中 `<region>` 为地域简称，例如 `cn-bj2`、`cn-sh2`、`cn-wlcb` 等，地域简称列表请参阅 [地域和访问域名](/ulogservice/region_domain_name)。

> **注意**：请求地址中的地域必须与日志主题所在地域保持一致，跨地域无法查询。

## 鉴权方式

兼容接口采用 HTTP Basic Auth 鉴权，使用 UCloud 账号（或子账号）的 API 密钥：

| 参数 | 说明 |
| -- | -- |
| 用户名 | API 密钥的**公钥**（PublicKey） |
| 密码 | API 密钥的**私钥**（PrivateKey） |

API 密钥可在 [UCloud 控制台 > 账号管理 > API 密钥](https://console.ucloud.cn/uaccount/api_manage) 中获取。

> **建议**：为兼容接口单独创建子账号，并仅授予其日志检索权限（`QueryULogServiceLog` 操作），避免使用主账号密钥带来的安全风险。

## 支持的接口

| 接口 | 说明 |
| -- | -- |
| `GET /<topic_id>/_search` | 检索日志，查询条件通过 URL Query 参数传递 |
| `POST /<topic_id>/_search` | 检索日志，查询条件通过请求体传递 |

路径中的 `<topic_id>` 为日志主题 ID，在兼容接口中扮演 Elasticsearch 中索引（Index）的角色，可在 [日志主题列表](https://console.ucloud.cn/ulogservice/topic) 中获取。

> **说明**：请求头需设置 `Content-Type: application/json`；兼容接口使用查询 DSL 时，仍以日志主题为单位查询，不支持 Elasticsearch 中多索引、别名等概念。

## 兼容的 DSL

### 时间字段

Elasticsearch 中使用 `@timestamp` 表示日志时间，日志服务中对应的系统保留字段为 **`__timestamp__`**。按时间范围过滤日志时，请使用 `range` 查询作用于 `__timestamp__` 字段，时间值为**毫秒级 Unix 时间戳**。

```json
{
  "range": {
    "__timestamp__": {
      "gte": 1774923995000,
      "lte": 1774924895000
    }
  }
}
```

### 查询 DSL

以下查询条件可用于 `query` 中，均要求目标字段已开启索引：

| 查询类型 | 说明 |
| -- | -- |
| `term` | 精确匹配单个值 |
| `terms` | 精确匹配多个值中的任意一个 |
| `match` | 匹配指定值 |
| `match_phrase` | 短语匹配 |
| `multi_match` | 在多个字段上匹配，支持 `best_fields` 等类型及 `lenient` 参数 |
| `range` | 范围匹配，常用于 `__timestamp__` 时间范围过滤 |
| `bool` | 组合条件，支持 `must`、`filter`、`should`、`must_not` |

### 聚合 DSL

聚合 DSL 分为**桶聚合**和**指标聚合**两类：桶聚合用于按字段值分组，指标聚合用于对分组内（或不分组时对整体）的数值字段做统计计算。

#### 桶聚合

| 聚合类型 | 说明 |
| -- | -- |
| `terms` | 按字段值分组统计，要求字段已开启索引**并开启统计**，支持 `size`、`min_doc_count` 等参数 |

#### 指标聚合

| 聚合类型 | 说明 |
| -- | -- |
| `min` | 统计最小值 |
| `max` | 统计最大值 |
| `avg` | 统计平均值 |
| `sum` | 统计总和 |
| `count` | 统计总数 |
| `cardinality` | 统计不重复的数据总数（去重计数） |
| `percentiles` | 统计百分位，支持 `percents`、`keyed` 等参数；`percents` 默认为 `[1, 5, 25, 50, 75, 95, 99]` |

> **说明**：
> - 指标聚合作用的字段需已开启索引**并开启统计**，否则无法返回预期结果。
> - 聚合结果与 Elasticsearch 一致，通过响应体的 `aggregations` 字段返回。

### 通用参数

| 参数 | 类型 | 说明 |
| -- | -- | -- |
| `from` | int | 分页起始位置，默认 0 |
| `size` | int | 返回日志条数，默认 100，最大 10000 |
| `sort` | object/array | 排序规则，排序字段需已开启索引**并开启统计** |
| `track_total_hits` | bool | 是否返回命中的日志总条数，取值为 `true` 时响应体的 `hits.total.value` 为精确值 |

## 使用示例

以下示例中的 `<region>`、`<topic_id>`、`<public_key>`、`<private_key>` 请替换为实际值。

### 示例 1：条件查询

查询北京时间 `2026-03-31 10:26:35` 至 `2026-03-31 10:41:35` 之间、请求方法为 `GET` 的日志，返回前 10 条并统计总条数：

```bash
curl -u "<public_key>:<private_key>" \
  -X POST "https://internal.<region>.uls.ucloud.cn/elasticsearch/<topic_id>/_search" \
  -H 'Content-Type: application/json' \
  -d '{
    "track_total_hits": true,
    "size": 10,
    "query": {
      "bool": {
        "filter": [
          { "match": { "reqMethod": "GET" } },
          {
            "range": {
              "__timestamp__": {
                "gte": 1774923995000,
                "lte": 1774924895000
              }
            }
          }
        ]
      }
    }
  }'
```

### 示例 2：全文检索 + 聚合分析

在上述时间范围内全文检索关键字 `USER`，并按 HTTP 状态码分组统计各状态码的日志数量：

```bash
curl -u "<public_key>:<private_key>" \
  -X POST "https://internal.<region>.uls.ucloud.cn/elasticsearch/<topic_id>/_search" \
  -H 'Content-Type: application/json' \
  -d '{
    "size": 0,
    "query": {
      "bool": {
        "filter": [
          {
            "multi_match": {
              "query": "USER",
              "type": "best_fields",
              "lenient": true
            }
          },
          {
            "range": {
              "__timestamp__": {
                "gte": 1774923995000,
                "lte": 1774924895000
              }
            }
          }
        ]
      }
    },
    "aggs": {
      "http_code_distribution": {
        "terms": {
          "field": "resHttpCode",
          "size": 100,
          "min_doc_count": 1
        }
      }
    }
  }'
```

响应结果的 `aggregations.http_code_distribution.buckets` 中即为各状态码及其对应的日志条数。

### 示例 3：分位数统计

统计上述时间范围内请求响应时间（`response_time`）的 P50、P90、P95、P99 分位数：

```bash
curl -u "<public_key>:<private_key>" \
  -X POST "https://internal.<region>.uls.ucloud.cn/elasticsearch/<topic_id>/_search" \
  -H 'Content-Type: application/json' \
  -d '{
    "size": 0,
    "query": {
      "bool": {
        "filter": [
          {
            "range": {
              "__timestamp__": {
                "gte": 1774923995000,
                "lte": 1774924895000
              }
            }
          }
        ]
      }
    },
    "aggs": {
      "response_time_percentiles": {
        "percentiles": {
          "field": "response_time",
          "percents": [50, 90, 95, 99]
        }
      }
    }
  }'
```

响应结果的 `aggregations.response_time_percentiles.values` 中即为各分位数及其对应的响应时间值，例如 `{"50.0": 12, "90.0": 58, "95.0": 132, "99.0": 476}`。

## 注意事项

- 兼容接口仅覆盖 Elasticsearch 的部分能力，写入类接口（如 `_bulk`、`_index`）、索引管理类接口（如 `_mapping`、`_cat`）均不支持，日志写入请使用 [HTTP 协议上报日志](/ulogservice/operate/http) 或 LogAgent 采集。
- 时间过滤必须使用 `__timestamp__` 字段，直接使用 `@timestamp` 无法命中日志。
- 未开启索引的字段无法检索；需要聚合或排序的字段还需开启统计，否则查询将无法返回预期结果。
- 单次查询返回的日志条数由 `size` 控制，**最大为 10000**，超出该值的部分需通过 `from`、`size` 分页获取，或收窄查询时间范围。
