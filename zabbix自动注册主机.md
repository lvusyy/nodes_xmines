# zabbix自动注册主机

1. zabbix-agent主机配置

 ```css
   PidFile=/var/run/zabbix/zabbix_agentd.pid
   LogFile=/var/log/zabbix/zabbix_agentd.log
   #LogFileSize=0
   Server=118.22.75.159
   ServerActive=118.22.75.159
   Hostname=xzhomersp2
   #UnsafeUserParameters=1
   HostMetadataItem=system.uname #注意需要增加这个参数
   Include=/etc/zabbix/zabbix_agentd.d/*.co
 ```

   2.zabbix-server 配置

   1) 新建一个Actions(动作)` 配置->动作->事件源(自动注册)->创建动作`

   ![1563092918520](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1563092918520.png)

   2)配置触发条件

   ​	自定义触发条件

   3)配置触发后操作

   ​	如链接模板,加入主机组等操作.

   

