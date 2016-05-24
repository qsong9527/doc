# Charles Https协议接口数据抓取
### 1. 配置Charles，启动SSL Proxy配置
#### 1.1 开启Charles的Proxy菜单，选择【SSL Proxying Settings...】选项
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/charles/Charles-https-settings-1.png)
#### 1.2 勾选【Enable SSL Proxying】选项，启用SSL Proxying
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/charles/Charles-https-settings-2.png)
#### 1.3 新增需要监控的SSL Locations配置，如果全部监控则为*，如上图
<br \>
### 2. 手机端安装SSL证书
#### 2.1 连接需要进行代理检测的设备至Charles服务器，默认8888端口。
#### 2.2 打开Charles Helper菜单，选择【SSL Proxying】选项卡下的【Install Charles Root Certificate on a Mobile Device or Remote Browser ...】
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/charles/Charles-https-settings-3.png)
#### 2.3 通过手机原生浏览器访问所弹出的选项卡中的地址，进行设备端证书安装
![image](https://raw.githubusercontent.com/zenist/doc/master/resource/charles/Charles-https-settings-4.png)
