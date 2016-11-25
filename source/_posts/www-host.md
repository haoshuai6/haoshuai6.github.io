---
title: 如何去掉WWW，使用顶级域名访问网站

---
拿我的域名为例：所谓顶级域名就是你申请的根域名.
1. 顶级域名(根域名)： hsblogs.com.
2. 二级域名：
   -  www.hsblogs.com  也是二级域名，只不过人们习惯用 `www.+域名` 而已.
   - `blog.hsblogs.com`  ， `xx.hsblogs.com`  都是二级域名.
3. 去域名服务商，做解析，例如阿里云万网.
   - 登录阿里云/万网【管理控制台】，点击【产品与服务】–【云解析】，进入域名解析列表；
   - 选择需要设置解析的域名点击【添加解析】，增加两条记录分别是主机记录（RR）为空或为@，主机记录（RR）为WWW，即可实现。
   - 例如：如果要将域名解析为 www.example.com，在主机记录（RR）处填写 WWW 即可；如需要直接解析为 example.com，主机记录（RR）处为空，或填写@。
4. 解析完成后，web服务器`apache`或者`nginx`也进行修改，拿`nginx`为例，在`nginx单ip单端口多域名的虚拟主机设置环境`下，在 `vhost`文件夹下，`vim    hsblogs.com.conf` 编辑.
![image](/img/20161124101140.png)
在原来的 `www.hsblogs.com.conf` 处修改 `rewrite` 如图：
```
rewrite ^/(.*)$ http://hsblogs.com/$1 permanent;
```
![image](/img/20161124101222.png)
**修改完成后，重启 nginx服务器即可。**

>注：如果是wordpress，更换顶级域名，就如同换了一个新域名，要在后台设置处，或者直接修改数据表

1. wp后台–>设置–>常规：
![image](/img/20161124101859.png)
2. 或者直接修改数据表：wp_options 
![image](/img/20161124101833.png)

>至此完工，以后访问 www.hsblogs.com  所有url 都会重定向到 hsblogs.com

---
5. 综上：实质就是`重定向`
    - 需求1：将不带www的顶级域名请求全部重定向到带www的二级域名
    - 需求2：反过来，将带www的二级域名请求全部重定向到不带www的顶级域名
    - 需求3：目录重定向
    - 需求4：目录跳转新域名
    
需求1：

```
server {
      server_name hsblogs.com;
      rewrite ^/(.*)$ http://www.hsblogs.com/$1  permanent;
 }
```
需求2：
```
server {
     server_name www.hsblogs.com;
     rewrite ^/(.*)$ http://hsblogs.com/$1 permanent;
 }
```
需求3：

```
if ( $request_filename ~  blog/ ) {
      rewrite  ^ http://www.hsblogs.com/newblog/?  permanent;
 }
```
需求3：

```
if ( $request_filename ~ blog/ ) {
      rewrite ^ http://blog.hsblogs.com/? permanent;
}
```


