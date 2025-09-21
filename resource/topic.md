# 主题管理

日志主题是日志数据在日志服务进行采集、存储、检索的基本单元，采集到的海量日志以日志主题为单元进行管理。

## 创建主题

登录 [ULogservice控制台](https://console.ucloud.cn/ulogservice/topic)，进入主题管理。

单击创建主题。

![创建主题1](/images/topic/create_topic_1.png)

填写主题名称、保存时间。

![创建主题2](/images/topic/create_topic_2.png)

**说明：有效期保存最长为360天**

## 日志接入

点击”日志接入“按钮，选择不同数据源进行日志接入。

![log_collect_config](/images/topic/log_collect_config.png)

## 检索分析
点击检索分析跳转至主题日志查询页面。

![检索导航](/images/topic/topic_navigate_1.png) 


## 查看采集配置

点击”查看采集配置“按钮可查看该主题已接入的采集配置信息，具体见[日志检索](/ulogservice/resource/search.md)。

![collect_rule_button](/images/topic/collect_rule_button.png) 

采集配置信息包括：
* 采集规则名称
* 日志源
* 关联资源
* 路径
* 提取模式
* 采集策略

![collect_rule_list](/images/topic/collect_rule_list.png) 

### 采集配置修改

选择指定的采集配置，点击”详情“按钮进入采集配置详情页。

![collect_rule_detail](/images/topic/collect_rule_detail.png) 

点击”编辑“按钮进行修改采集配置规则。

![collect_rule_update](/images/topic/collect_rule_update.png) 

### 采集配置删除

当指定采集配置日志不再收集时，可删除该采集配置，点击列表”删除“按钮进行删除。规则删除之后对应日志不再继续收集。

![collect_rule_delete](/images/topic/collect_rule_delete.png) 

## 查看索引配置

选择指定的采集配置，点击”查看索引配置“按钮查看已配置索引信息。

![log_index_detail](/images/topic/log_index_detail.png) 

### 修改索引配置

点击”编辑“按钮进行索引配置修改。

![log_index_detail](/images/topic/log_index_info.png) 


![log_index_update](/images/topic/log_index_update.png) 

* 索引启用状态：关闭索引后将无法检索和查询采集到的日志，请谨慎操作。
* 索引字段：点击开启索引状态，并添加或删除字段。
   | 字段数据类型 | 说明         |
   | ------------ | ---------- |
   | text         | 支持字段搜索 |
   | long         | 支持范围比较 |
   | double       | 支持范围比较 |

说明：系统默认字段不支持修改与删除。
  
## 主题编辑

修改主题保存时间。
![主题编辑](/images/topic/topic_edit_1.png)

## 删除主题
 在主题列表中，找到需要删除的主题，在右侧操作列表中点击删除。

![主题删除](/images/topic/topic_delete_1.png)

**说明：主题删除后，数据无法恢复**

