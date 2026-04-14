# 机器组管理

机器组是日志服务（ULS）中LogAgent所采集日志的服务器对象，通过机器组的方式来配置LogAgent的日志采集。可以根据不同的业务划分不同的机器组，方便管理日志服务。

## 创建机器组
### 通过机器标识创建机器组
1. 登录 [ULogservice控制台](https://console.ucloud.cn/ulogservice/machinegroup)，进入机器组管理。

2. 单击创建机器组。

![创建机器组1](/images/machinegroup/create_machinegroup_1.png)

3. 填写机器组名称、机器组类型。

![创建机器组2](/images/machinegroup/create_machinegroup_2.png)

* 机器组名称：例如uls_test。
* 机器组类型：选择机器标识。
* 机器标识：输入标识，拥有该标识的LogAgent会自动加入到当前机器组中。LogAgent设置标识请参考[LogAgent安装指南（Linux 版）](/operate/logagent_install.md)。

4. 单击确定。
5. 登录配置了机器标识的目标机器，并执行如下命令，打开LogAgent安装目录下的filebeat.yml。
```
vim /usr/local/logagent/filebeat.yml
```
6. 按下i键进入编辑模式。
7. 找到group_labels参数，填写您自定义的机器标识。
![填写机器标识](/images/machinegroup/group_labels.png)
8. 按下Esc键，退出编辑模式，输入:wq，并按下Enter键，保存设置。
9. 执行如下命令，重启LogAgent，即可完成创建。
```
systemctl restart logagent
```
### 通过关联机器IP地址创建机器组
1. 登录 [ULogservice控制台](https://console.ucloud.cn/ulogservice/machinegroup)，进入机器组管理。

2. 单击创建机器组。

![创建机器组3](/images/machinegroup/create_machinegroup_1.png)

3. 填写机器组名称、机器组类型。

![创建机器组4](/images/machinegroup/create_machinegroup_3.png)

* 机器组名称：例如uls_test。
* 机器组类型：选择机器组IP地址。
* 机器组IP地址：输入需要加入机器组的机器IP地址。

4. 单击确定，即可完成创建。

## 查看机器状态
1. 登录 [ULogservice控制台](https://console.ucloud.cn/ulogservice/machinegroup)，进入机器组管理。
2. 在右侧操作栏点击详情
![查看机器状态](/images/machinegroup/view_machinegroup_1.png)
3. 在弹出的窗口中，即可查看机器组状态
![查看机器状态](/images/machinegroup/view_machinegroup_2.png)
4. 通过查看机器组的状态可识别当前该机器是否工作正常。若状态显示正常，则说明您的服务器可以与日志服务正常通信。

> 注意：
> 当LogAgent离线超过2分钟，机器状态会显示成：“心跳异常”。离线时间超过7天，LogAgent会被自动剔除。

## 删除机器组
1. 登录 [ULogservice控制台](https://console.ucloud.cn/ulogservice/machinegroup)，进入机器组管理。
2. 在右侧操作栏点击删除。

![删除机器组1](/images/machinegroup/delete_machinegroup_1.png)
3. 在弹出的提示窗口，单击确认，完成机器组删除。
> 注意：
> 机器组一旦删除，所有管理的日志主题就无法继续采集日志。

![删除机器组2](/images/machinegroup/delete_machinegroup_2.png)