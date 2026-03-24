# 日志上报概述
日志服务支持通过HTTP协议进行日志上报，可以直接在客户端中发送HTTP请求到日志服务中。

# 协议内容
## Headers
| 参数名 | 类型 | 描述信息 | 必填 |
| -- | -- | -- | -- |
| X-Action | string | 固定值：`PutULSLog` | 是 |
| X-ULS-PublicKey| string | PublicKey 公钥 | 是 |
| X-ULS-Signature | string | 请求签名，详情参考下方的签名算法 | 是 |

## Body
| 参数名 | 类型 | 描述信息 | 必填 |
| -- | -- | -- | -- |
| Topic | string | uls主题ID | 是 |
| LogGroup | Log类型的数组 | 日志内容的数组 | 是 |
| Source | string | 日志来源标识 | 否 |

### Log
| 参数名 | 类型 | 描述信息 | 必填 |
| -- | -- | -- | -- |
| Timestamp | int64 | 日志时间戳，不传则使用接收时间 | 否 |
| FilePath | string | 日志文件路径 | 否 |
| LogContent | LogContent类型的数组 | 日志字段数组 | 是 |

### LogContent
| 参数名 | 类型 | 描述信息 | 必填 |
| -- | -- | -- | -- |
| Key | string | 字段名 | 是 |
| Value | string | 字段值 | 是 |

# 签名算法
```
Signature = HMAC-SHA1(UCloudPrivateKey,UTF-8-Encoding-Of(StringToSign)) 
```

## StringToSign
```
StringToSign = HTTP-Verb + "\n" +
    Content-MD5 + "\n" +
    Content-Type + "\n" +
    CanonicalizedHeaders
```