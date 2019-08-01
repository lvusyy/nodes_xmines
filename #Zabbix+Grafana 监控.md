#Zabbix+Grafana 展示示例



> Grafana是一个跨平台的开源度量分析和可是化的工具,可以通过该将采集的数据查询然后可视化的展示,并及时通知.

#### 1. Grafana 特性

```css
1. 展示方式:快速灵活的客户端图表,面板插件有许多不同方式的可视化指标和日志,官方库中具有丰富的仪表盘插件,比如热图,折线图,图表等多种展示方式.

2. 数据源: Graphite,InfluxDB,OpenTSDB,Prometheus,Elasticsearch,CloudWatch和KairosDb,Zabbix等.

3. 通知提醒:以可视方式定义最重要指标的报警规则,Grafana将不断计算并发送通知,在数据达到预设阈值时通过slack,PagerDuty等处理通知.

4. 混合展示: 在同一图表中混合使用不同的数据源,可以基于每个查询指定数据源,甚至自定义数据源.

5. 注释: 使用来自不同数据源的丰富事件来展示图表,将鼠标悬停在事件上会显示完整的事件元数据和标记.

6. 过滤器:Ad-hoc过滤器允许动态创建新的键/值过滤器,这些过滤器会自动应用于使用该数据源的所有查询.


```

### 2. Grafana 下载和安装

#### 2.1.1 Redhat & Centos 的安装

> Redhat & Centos(64 Bit)SHA256: eb632b9013c8b53ff06080298e57e5b0c47b4f7fcefb6af44fe84b0e63aad182

```bash
wget https://dl.grafana.com/oss/release/grafana-6.2.5-1.x86_64.rpm 
sudo yum localinstall grafana-6.2.5-1.x86_64.rpm 
```
#### 2.1.2 Ubuntu & Debian 的安装
> Ubuntu & Debian(64 Bit)SHA256: a095fca6240e0edb9da03d01ba6f47fd7dccc01472db64ece025b27874d2b827

```bash
wget https://dl.grafana.com/oss/release/grafana_6.2.5_amd64.deb 
sudo dpkg -i grafana_6.2.5_amd64.deb 
```

### 2.2 Grafana 配置和启动

#### 2.2.1 配置

设置自启动

`systemctl enable grafana-server`

启动服务

`systemctl start grafana-server`

查看服务是否启动

`ps -ef|grep grafana`

```css
# ps -ef|grep grafana
grafana   8092     1  1 13:54 ?        00:00:00 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana-server.pid --packaging=rpm cfg:default.paths.logs=/var/log/grafana cfg:default.paths.data=/var/lib/grafana cfg:default.paths.plugins=/var/lib/grafana/plugins cfg:default.paths.provisioning=/etc/grafana/provisioning

```

查看端口是否监听

`ss -anlpt |grep 3000`

```css
# ss -anlpt |grep 3000
LISTEN     0      128         :::3000                    :::*                   users:(("grafana-server",pid=8092,fd=8))

```

登陆Grafana 

使用浏览器打开 `http://服务器ip:3000` 

默认账号:`admin` 密码:`admin`

![1562997497155](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1562997497155.png)

第一次要求更改密码,可以直接`skip` 跳过.

默认主页面

![1562997639301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1562997639301.png)



### 3. 安装zabbix插件

#### 3.1 grafana-cli plugins install 方式安装

​	*由于国内网速原因下载特别的慢...无语的那种慢*

​	`grafana-cli plugins install alexanderzobnin-zabbix-app`

#### 3.2 使用git clone 方式(经测试这种方法在插件页面会一直loading)

```bash
cd /var/lib/grafana/plugins
yum install -y git
git clone --depth=1  https://github.com/alexanderzobnin/grafana-zabbix
```

  程序启动服务

`systemctl restart grafana-server`

#### 3.2.1 下载解压方式安装插件

```css
(base) [root@centos plugins]# proxychains4 grafana-cli plugins install alexanderzobnin-zabbix-app
installing alexanderzobnin-zabbix-app @ 3.10.2
from url: https://grafana.com/api/plugins/alexanderzobnin-zabbix-app/versions/3.10.2/download
into: /var/lib/grafana/plugins
```

上面信息告诉我们从哪里下载  https://grafana.com/api/plugins/alexanderzobnin-zabbix-app/versions/3.10.2/download

以及下载到哪里去 `/var/lib/grafana/plugins`

我们下载到`/var/lib/grafana/plugins` 然后直接解压出来重新启动服务`systemctl restart grafana-server` 即可.

 

### 4. 添加zabbix数据源

![1563034917287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1563034917287.png)

保存测试...

之后导入自带的3个Dashboards

查看路径. dashboard-manager 就能看到自带的模板案列.