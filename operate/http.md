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
HTTP-Verb: 固定的POST方法<br />
Content-MD5: 对http body部分使用md5计算得到的<br />
Content-Type: application/json<br />
CanonicalizedHeaders:<br/>
```
1. 将每个以X-UCloud-开头的 HTTP 标头名称转换为小写。例如X-UCloud-Date改为x-ucloud-date。
2. 根据标头名称按字典顺序排列标头集。
3. 按照 RFC2616 中第 4.2 节中的规定，将相同名称的标头字段合并为一个header-name:comma-separated-value-list对，各值之间不留空格。
4. 例如，可以将元数据标头x-ucloud-meta-username:fred和x-ucloud-meta-username:barney合并为单个标头x-ucloud-meta-username:fred,barney。
5. 通过将折叠空格（包括换行符）替换为单个空格，“展开”跨多个行的长标头（按照 RFC2616 中第 4.2 节允许的方式）。
6. 删除标头中冒号周围的空格。例如，标头x-ucloud-meta-username: fred,barney改为x-ucloud-meta-username:fred,barney。
最后，请向生成的列表中的每个标准化标头附加换行字符（U+000A）。通过将此列表中所有的标头规范化为单个字符串，构建 CanonicalizedUcloudHeaders 元素。
```
### 示例
```
StringToSign="POST\n53cf0911f25edc439cfe1316e8c68055\napplication/json\nx-ucloud-foo:foo"
```
上述示例的各个字段的值如下：
HTTP-Verb:POST<br />
Content-MD5:53cf0911f25edc439cfe1316e8c68055<br />
Content-Type:application/json<br />
CanonicalizedHeaders:x-ucloud-foo:foo<br />