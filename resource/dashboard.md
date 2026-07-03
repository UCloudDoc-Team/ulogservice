# Dashboard
Dashboard 是日志服务提供的可视化日志监控功能。通过 Dashboard ，您可以将多个日志主题的检索分析结果以图表形式集中展示，实现日志数据的实时监控和趋势分析。

## 基本概念
### 大盘
大盘是图表的容器，用于组织和展示一组相关的监控图表。您可以：
- 为不同业务场景创建独立的大盘
- 在同一个大盘中展示多个日志主题的检索分析图表
- 在大盘级别设置时间范围，影响所有图表的数据查询

### 图表组
图表组是大盘内的分组容器，用于将相关图表归类展示。

### 图表
图表是大盘的核心元素，每个图表关联一个检索分析语句：
- **数据来源**：指定日志集和主题
- **检索分析语句**：使用类SQL语法查询和聚合数据
- **图表类型**：时序图、柱状图或表格

## 操作指南
### 大盘管理
1. 创建大盘：在 Dashboard 列表页，点击 `创建大盘`。填写大盘名称（最长 128 字符），点击确定完成创建。
![创建大盘1](/images/dashboard/create_dashboard1.png)
![创建大盘2](/images/dashboard/create_dashboard2.png)
2. 修改大盘：在 Dashboard 列表页和详情页，点击编辑按钮，修改大盘名称，点击确定保存修改。修改大盘名称不影响已创建的图表组和图表。
![修改大盘1](/images/dashboard/update_dashboard1.png)
![修改大盘2](/images/dashboard/update_dashboard2.png)
![修改大盘3](/images/dashboard/update_dashboard3.png)
3. 删除大盘：在 Dashboard 列表页，点击 `删除`，确认删除大盘。删除大盘前需要删除其中所有图表组。
![删除大盘1](/images/dashboard/delete_dashboard1.png)
![删除大盘2](/images/dashboard/delete_dashboard2.png)

### 图表组管理
1. 创建图表组：在大盘详情页，点击 `添加图表组`。填写图表组名称（最长 128 字符），点击确定完成创建。
![创建图表组1](/images/dashboard/create_chartgroup1.png)
![创建图表组2](/images/dashboard/create_chartgroup2.png)
2. 修改图表组：点击图表组的编辑按钮，修改图表组名称，点击确定保存。修改图表组名称不影响组内图表。
![修改图表组1](/images/dashboard/update_chartgroup1.png)
![修改图表组2](/images/dashboard/update_chartgroup2.png)
3. 删除图表组：点击图表组的删除按钮，确认删除。删除图表组前需要删除其中所有图表。
![删除图表组](/images/dashboard/delete_chartgroup.png)

### 图表管理
#### 创建图表
1. 在大盘详情页，点击 `添加图表`
![添加图表](/images/dashboard/create_chart1.png)
2. **填写查询内容**：选择日志集和主题，输入检索分析语句，点击`查询`，右侧实时显示图表。
3. **选择图表类型**：
   - 时序图：适合展示指标随时间变化的趋势
   - 柱状图：适合展示不同分类的数值对比
   - 表格：适合查看详细数据
4. **配置图表字段**（时序图/柱状图）：
   - X 轴字段：时序图仅支持选择 time 类型，柱状图仅支持选择 text 类型
   - Y 轴字段：支持一个或多个 long/double 类型字段
5. **设置数据上限**：
   - 时序图/表格：默认展示 100 条数据，最大可修改为 10000 条。
   - 柱状图：默认展示 20 条数据，最大可修改为 100 条。
6. **填写基础信息**：
   - 图表名称（最长 128 字符）
   - 所属分组
7. 点击 `保存` 完成创建
![添加图表](/images/dashboard/create_chart2.png)

#### 修改图表
1. 点击图表右上角的 **编辑** 按钮
![修改图表](/images/dashboard/update_chart.png)
2. 修改图表配置（支持修改所有配置项，操作与创建图表相同）
3. 点击 `保存` 完成修改

#### 删除图表
1. 点击图表右上角的 **删除** 按钮
2. 确认删除操作
![删除图表1](/images/dashboard/delete_chart1.png)
![删除图表2](/images/dashboard/delete_chart2.png)

#### 调整图表顺序
1. 拖拽图表到目标位置（支持图表组内/跨组移动，支持图表组排序）
2. 点击 `保存` 完成修改
![调整图表顺序](/images/dashboard/update_chart_order.png)

## 检索分析语句说明

### 基本语法

日志服务检索分析语句由两部分组成：`[检索条件] | [SQL 语句]`

- **检索条件**：用于过滤日志，支持关键词搜索和字段条件，详情请参阅 [检索语法](/ulogservice/operate/syntax_search)
- **SQL 语句**：用于聚合分析，支持类 SQL 语法，详情请参阅 [SQL语法](/ulogservice/operate/syntax_analysis)

**示例**：
```sql
level:error | select count(*) as error_count
```
查询包含 "error" 关键词的日志数量。

### 时序图
时序图的检索分析语句中需要包含至少一个时间类型的字段，用于X轴展示。
更多时间函数请参阅 [日期和时间函数](/ulogservice/operate/analysis_func/date_and_time)

**示例**：
```sql
* | SELECT histogram(__TIMESTAMP__, 'INTERVAL ${__interval}') as time_bucket, count(*) GROUP BY time_bucket ORDER BY time_bucket
```
统计动态时间间隔中上报的日志条数。

### 柱状图
柱状图的检索分析语句中需要包含至少一个字符串类型的字段，用于X轴展示。
更多字符串函数请参阅 [字符串函数](/ulogservice/operate/analysis_func/string)

**示例**：
```sql
* | SELECT __source_host__, count(*) GROUP BY __source_host__
```
统计每个数据源上报的日志条数。