今天腾出了一些时间，把搭建博客的过程记录一下，也给之后想搭建的旁友们提供一些方便。对搭建博客来说因为以前用的是基于github的，所以对服务器的知识也是一无所知，这次完完全全是从入门开始一点一点学习起来的:nerd:。
## 购买服务器
 - 首先是要购买一个服务器，找了腾讯云、阿里云、金山云等国内的服务器，发现价格是差不多的，但是国内的都要备案，好吧我只知道这些，所以听劝买了国外的[<u>***Vultr***</u>](https://www.vultr.com/)服务器，第一个不用备案，另外价格上也还是比较说得过去的（可以用支付宝的）。![alt](http://hattiewen.top/upload/2018/02/plre3djhh2j3po3mac24ikha05.png)
（选择东京或者新加坡的就可以，距离比较近。）
 - 下一个服务器类型我选的是Ubuntu17.10，这个看自己哪个比较熟悉就可以。
![alt](http://hattiewen.top/upload/2018/02/2a4qudhsciipmrkp7qbskj3rio.png)
 - 价格当然选的是最少的，但是最少的已经售空了，选了$5/month的。
![alt](http://hattiewen.top/upload/2018/02/010fstgr4uhbhp3mf43ubcfij2.png)
 - Additional Features可以不用选，Startup Scripts也先不用选。SSH免密登录，可以不选，如果需要的话可以学习一下大佬的[<u>***这篇***</u>](https://zhuanlan.zhihu.com/p/28423720)文章。
![alt](http://hattiewen.top/upload/2018/02/1gihqjv28ijneofmdhrg6190kl.png)
 - 最后的Firewall防火墙可以暂时先不选，之后再设置。Server Hostname & Label可以不填。
![alt](http://hattiewen.top/upload/2018/02/riblek9f5ajt1rhp314o4teo9m.png)
 - OK！到这里，点击确认购买，服务器就买好咯。在Server菜单中查看自己的服务器状态，连接密码等信息。如下图所示：可以启动、关闭或重装服务器，那串数字就是你要连接的IP地址啦。小眼睛部分隐藏的是你的连接密码。
![alt](http://hattiewen.top/upload/2018/02/ts3ngni4k2glfo5392kk0lnhi6.png)
![alt](http://hattiewen.top/upload/2018/02/vtrapoob9kh9jouarqn7o8vhsv.png)
下面就要开始连接我们的服务器了，才可以进行安装tale博客系统。在终端中输入如下命令：
```
ssh vultr   
```  
然后就进入了服务器的系统。
![alt](http://hattiewen.top/upload/2018/02/11l5e4svaaihjo5avkhp9cjl1m.png)

## 安装JDK
 - 直接依次输入以下命令即可，不详细介绍
```
sudo apt-get update
sudo apt-get install default-jre
java -version
```  

## 安装Tale
 - 就到了安装tale博客系统的阶段了，在Linux中使用wget命令进行安装，比较方便。依次输入下列命令，安装后解压，然后定位到tale的文件夹中，最后一个命令便是启动tale博客系统。
```
wget http://static.biezhi.me/tale-least.zip
apt-get install zip unzip
unzip tale-least.zip
cd tale
chmod +x tale-cli
./tale-cli start
```  
安装成功并启动后应该显示介个样子。
![alt](http://hattiewen.top/upload/2018/02/03b4t3b1bugj9rs0743rqfvtog.png)
这时，在浏览器地址栏中输入你服务器的IP地址后面加上你开放的端口号（一般是9000），显示的界面应该就是安装tale的界面。
![alt](http://hattiewen.top/upload/2018/02/t3acqdtstcjddp7jdnadpeutai.png)
 - 但是在这里我们先不要点安装，因为这时安装是用IP加端口的方式安装的，不是我们指定的域名，可以购买一个域名后，将域名指向这个IP地址，再进行安装。

## 域名设置
 - 首先购买一个域名，我是在阿里云购买的top结尾的域名，原因就很重要了，那就是便宜！top、xyz等结尾的域名都是国际通用的，可以自行选择购买域名类型。
 - 在Linux中安装一个nginx，过程也是很简单的。执行以下命令。安装nginx，然后启动nginx。
```
apt-get install nginx
sudo service nginx start
```  
 - 然后要做的就是把域名指向IP，对域名进行DNS解析，添加一条记录，“类型”:A，“值”：IP地址。其他不用变。ps：我这里登录出了点问题，就先不截图啦。
 - 添加好记录之后，执行以下命令。定位到nginx的文件夹中，
```
cd /etc/nginx
cd conf.d/
touch tale.conf
vim tale.conf
```  
vim是对新创建的tale.conf进行编辑，在其中加入你的指向信息：
```
server {
  listen 80;
  server_name 你的域名;
  location / {
    proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_pass http://127.0.0.1:9000;
		client_max_body_size 10m;
  }
}
```  
把“你的域名”替换为你申请好的域名就行了。然后保存退出。（vim的操作可以自行百度一下）
 - 然后重启一下nginx：
```
sudo service nginx restart
```  
 - 这个时候我们的域名和IP就绑定成功了，可以输入自己的域名，进行tale的安装。

## tale博客的使用
 - 安装后进入的是管理员的界面，设置名称、密码等。
 - 具体的操作我就不一一介绍了，还有很多都值得自己去探索，大家可以参考[<u>这里</u>](https://github.com/otale)，或者[<u>biezhi大佬</u>](https://github.com/biezhi)的git。

至此，就可以愉快的使用自己的博客啦！我也是第一次，如果文中有什么不合理的地方还请各位旁友指正哦！如果有什么问题可以给我留言，我会尽自己所能去解决的！
