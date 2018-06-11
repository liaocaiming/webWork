**weinre简介**
********
weinre 是一款类似于firebug 和Web Inspector的网页调试工具， 它的不同之处在于可以用于进行远程调试，比如调试手机上面的网页。

**weinre的安装**
***

```
npm install -g weinre
```
**如何运行?
**
安装完成后在termial中运行以下命令
```
weinre --httpPort=8081 --boundHost=www.liaocaiming.site //你自己Debug Server的域名， 也可以使用ip地址)
```

打开浏览器， 输入 http://www.liaocaiming.site:8081
![](/assets/weinre.png)

在需要调试的网页中加入Target Script
```
<script src="http://znchen.waijule.cn:8081/target/target-script-min.js#anonymous" />
```

打开debug client 界面
```
http://www.liaocaiming.site:8081/client/#anonymous
```

