# Grafana +Zabbix 系列二

## Grafana 简介补充

+ Grafana自身并不存储数据,数据从其他地方获取.需要配置*数据源*

+ Grafana支持从Zabbix中获取数据

+ Grafana优化图形的展现,可以用来做监控大屏.

+ Grafana支持用户认证,增加运维安全性.

  关于Grafana权限:

  Grafana 的权限分为三个等级：Viewer、Editor 和 Admin，Viewer 只能查看 Grafana 已经存在的面板而不能编辑，Editor 可以编辑面板，Admin 则拥有全部权限例如添加数据源、添加插件、增加 API KEY。

### Grafana使用的补充

* 需要先创建dashboard,然后在创建图形
* 每次操作都需要save保存.
* Grafana数据源是指明Grafana获取数据的来源.默认支持influxdb等
* Zabbix不在默认支持数据源中,我们可以使用安装插件的方式来扩充此功能.

### Grafana安装插件一些补充

	>小提示:
	>
	> grafana默认存放插件的目录是`/var/lib/grafana/plugins`
	>
	>安装完插件需要重新启动grafana服务来让插件正确启用.

一些安装命令:

+ grafana-cli plugins list-remote                                            #列出可用插件
+ grafana-cli plugins list-remote |grep zabbix                    #查看zabbix 插件是否在插件库中
+ grafana-cli plugins install alexanderzobnin-zabbix-app #安装最新的zabbix插件
+ frafana-cli plugins update-all                                               #更新所有插件
+ grafana-cli pluigins remove                                                  # 移除一个插件

更多关于grafana命令 https://grafana.com/docs/plugins/installation/#grafana-cli-commands



## grafana开启zabbix插件的路径

`plugins->apps->zabbix->enable`

## grafana配置文件

更多关于Grafana配置文件的信息 请关注官方帮助文档 https://grafana.com/docs/administration/provisioning/#config-file

`vim /etc/grafana/grafana.ini`



## Grafana 变量功能

#### Grafana变量的使用

* 主机组变量: group                *
* 主机变量:     host                  $group *
* 应用变量:     application       $group.$host.*
* 对象变量:      item                  $group.$host.$application.*

Grafana



![virables](C:\Users\Administrator\Documents\virables.png)

![1563173271303](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1563173271303.png)

![1563173330192](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1563173330192.png)





`Variable `
name: 变量名，template的名字，比如我这里取名为group，到时候要使用这个变量名就用$group来调用。 

`type`: 变量类型，变量类型有多种，其中query表示这个变量是一个查询语句，type也可以是`datasource`，`datasource`就表示该变量代表一个数据源，如果是`datasource`你可以用该变量修改整个`DashBoard`的数据源，变量类型还可以是时间间隔`Interval`等等。这里我们选择query。 

`label`: 是对应下拉框的名称，默认就是变了名，选择默认即可。 
`hide`: 有三个值，分别为空，`label`，`variable`。选择label，表示不显示下拉框的名字。选择`variable`表示隐藏该变量，该变量不会在`DashBoard`上方显示出来。默认选择为空，这里也选默认。

`Query options `
`Data source`: 数据源，不用多说 选`zabbix`

`Refresh`: 何时去更新变量的值，变量的值是通过查询数据源获取到的，但是数据源本身也会发生变化，所以要时不时的去更新变量的值，这样数据源的改变才会在变量对应的下拉框中显示出来。

`Refresh`有三个值可以选择，`Never`：永不更新。`On Dashboard Load`：在DashBoard加载时更新。`On Time Range Change`：跟随面板刷新时间刷新该变量，面板的刷新设置在面板的右上角。此处，选择`On Dashboard Load`

`Query`：查询表达式，不同的数据源查询表达式都不同（这些可以到官网上查询），这里由于是要查询zabbix的groups信息，所以表达式为*。 
Regex：正则表达式，用来对抓取到的数据进行过滤，这里默认不过滤。 
Sort：排序，对下拉框中的变量值做排序，排序的方式挺多的，默认是disable，表示查询结果是怎样下拉框就怎样显示。此处选disable。

`Selection Options `
`Multi-value`：启用这个功能，变量的值就可以选择多个，具体表现在变量对应的下拉框中可以选多个值的组合。 
`Include All option`：启用这个功能，变量下拉框中就多了一个all选项。 
`Custom all value`：启用Include All option这个功能，才会出现Custom all value这个输入框，表示给all这个选项自定义一个值，all这个选项默认是所有值的组合，你也可以自定义，比如我自定义all为cpu五分钟平均负载，则选择all就代表cpu五分钟平均负载。 
虽然选择组合值可以在一个panel里面查看多种监控数据，但是由于不同监控数据的数值大小格式都可能不一样，在一个图形里面格式很难兼容，这样就会出现问题，所以此处建议默认都不选。

`Value groups/tags `
组合标签，可以选择多个值组合在一起设置一个标签，这个功能还没弄明白，按照官网上的操作没效果，暂且忽略吧，反正没多大影响。

`Preview of values (shows max 20) `
前面都设置好之后，下方会显示前二十，由于我的groups只有一个zabbix agents，所以只显示了一个。





参考:

https://www.jianshu.com/p/fe8bab704716

https://www.jianshu.com/p/fe8bab704716

https://blog.csdn.net/u012062455/article/details/79229603