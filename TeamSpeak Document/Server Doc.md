# TeamSpeak服务器端配置

 ### Docker

在Docker中部署TeamSpeak,部署完成后，在部署日志中获取管理token。

在云服务器防火墙管理面板放行端口

| 协议 | 端口范围 |      备注      |
| :--: | :------: | :------------: |
| UDP  |   9987   |    TS-VOICE    |
| TCP  |  30033   |    TS-FILE     |
| TCP  |  41144   |     TS-DNS     |
| TCP  |  10011   | TS-ServerQuery |

[TeamSpeak官方接口文档](https://support.teamspeak.com/hc/en-us/articles/360002712257-Which-ports-does-the-TeamSpeak-3-server-use)



### 域名解析

在DNS解析界面添加如下规则，即可使用域名访问服务器

|  主机记录  | 记录类型 |        记录值        |
| :--------: | :------: | :------------------: |
|     ts     |    A     |     服务器IP地址     |
| \_ts3._udp |   SRV    | 0 5 9987 ts.域名地址 |



### TeamSpeak客户端

打开TeamSpeak客户端

输入域名加入服务器，在添加管理员密钥填入部署服务器后获取的管理token





