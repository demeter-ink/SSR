
#### VPN是什么
VPN，全称：Virtual Private Network，中文翻译：虛拟私人网络。
作用：提供安全可靠的通信渠道，一般而言企业使用较多。
延伸作用：科学上网。
说明：VPN的出现并不是为了“科学上网”，二是在公网上建立加密的通信渠道。例如，公司员工出差或者在寝室，想要登录公司内网邮箱怎么办?这时VPN就派上用场了，可以通过第三方连接工具进行远程连接，比如思科就有相应的工具。

#### 何为SS
SS全称shadowsocks，一开始为个人独立开发并用作“科学上网”，后被大家所熟知和广泛使用。再后来，据说作者被请去“喝茶”，停止了该项目。

#### 什么是SSR
SSR全称shadowsocks-R。SSR作者声称SS不够隐匿，容易被防火墙检测到，SSR在改进了混淆和协议，更难被防火墙检测到。简单地说，SSR是SS的改进版。

#### VPN与SSR、SS的区别?
SS和SSR两者原理相同，都是基于socks5代理。客户端与服务端没有建立专有通道，客户端和实际要访问的服务端之间通过代理服务器进行通信，客户端发送请求和接受服务端返回的数据都要通过代理服务器。
SSR目的是为了能让流量通过防火墙。
客户端请求服务端数据流程(SSR)：
(1)浏览器发送请求(基于socks5协议)， 通过ssr客户端将sock5协议通过协议插件和混淆插件进行转换加密，使得来自客户端的流量和基于HTTP协议的流量无差别;
(2)SSR服务端(代理服务器)收到请求后，通过混淆插件、协议插件将数据解密并还原协议，最后转发到目标服务器。
服务端返回数据到客户端同理。

## 建立自己的SSR服务器
#### 买服务器
这里以阿里云为例，购买
在阿里云的管理控制台上
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208150030185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
进入之后点击左边的“实例”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208150140566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208150443136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208150900315.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
滑动到下方继续配置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208151216750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208151540589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208151853679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208152005428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208152133787.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208152644215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

下面使用mac连接
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208153028665.png)

连接成功后在云主机上执行如下命令： 
```
wget --no-check-certificate https://freed.ga/github/shadowsocksR.sh; bash shadowsocksR.sh
```

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208154226973.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
命令执行后会显示服务端的连接信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208154459752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

***下面我们去开放阿里云的端口，端口不开通无法访问***

#### 设置阿里云开放端口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208154739659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208154853961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208155158994.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208155243209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208155518559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
## 客户端连接
* 客户端我放到附件中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208155750170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
输入刚才我们得到的SSR连接地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191208155946961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
* 请求google.as成功测速一下
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019120816041566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW80NF9qYXZh,size_16,color_FFFFFF,t_70)
