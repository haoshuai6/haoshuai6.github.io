---
title: Hexo+github绑定域名(主或二级)

---

 你申请的Github Page的域名是`username`.github.io ,是一个二级域名。
> 那么，我要想用自己买的域名，或者自己的二级域名，去访问Github Page呢？

> 要绑定域名到，你申请的Github Pages上


###  下面记录操作步骤：

#### 域名解析设置:

-  前提你必须有自己的域名，而且是通过备案的域名哦
-  因为我是在阿里云`万网`购买的域名，而且我的主域名 `www.hsblogs.com` 是我的主博客地址，所以只能新建个子域名  `github.hsblogs.com` 来测试啦，
-  进入域名管理控制台、域名解析设置，添加解析，如图
- 添加解析需要设置：
![image](/img/20161029140750.png)

>1. 记录类型：
要将域名指向主机服务商提供的IP地址，请选择「A记录」；要将域名指向主机服务商提供的另一个域名，请选择「CNAME记录」。

>2. 主机记录：
二级域名 ：
如：mail.example.com或abc.example.com，填写mail或abc；

> 3. 解析线路：一般默认

> 4. 记录值：
A记录：
将域名指向一个IPv4地址（例如：10.10.10.10），需要增加A记录
CNAME记录：
如果将域名指向一个域名，实现与被指向域名相同的访问效果，需要增加CNAME记录

综上所述，
- 你要是选择`「A记录」`，那么 `记录值` 你需要填写`github pages` 的IP地址，通过`ping`获取IP地址：
![image](/img/20161029140441.png)
得到IP地址 `151.101.100.133`,将   ip地址填入`记录值`，保存。

- 你要是选择 `「CNAME记录」` ，那么 `记录值` 你需要填写你的`github pages` 的域名，也就是 `username`.github.io ，保存
- 域名解析设置完了

#### Hexo设置
在 `haoshuai6.github.io / source` 目录下，新建名为 `CNAME` 的文件，`注意没有任何后缀`，打开文件编辑内容为你刚才新加解析的域名，`注意没有http://`，形如`github.hsblogs.com` 这样，保存。
![image](/img/20161029113722.png)
>设置完毕，`hexo`重新生成、部署到github即可！

等待几分钟，访问你刚申请的域名`github.hsblogs.com`即可跳转到`username`.github.io

