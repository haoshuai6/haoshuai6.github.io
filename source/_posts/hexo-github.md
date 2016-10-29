---
title: Hexo+Github搭建个人博客[两个分支方便维护]

---
一个高逼格的博客 `Hexo + github` ,这个组合免费、静态化、部署方便！编写文章使用Markdown，这可真是，很吸引人啊！

下面记录安装、搭建[Hexo + github]的过程

## 整个过程分为三部分

1. 远端部分 `github` 建库
2. 本地搭建 `Hexo` 博客
3. 关联，部署到 `github`

## Github建库

- 登陆github, 新建Create a new repository, 命名必须是你的github账户名 `username`.github.io ,这也就是你博客的域名啦！ 

## 本地部分： 

#### 环境准备

- 安装 [GIT](https://git-scm.com/downloads/) 或[ Github For Windows](https://desktop.github.com/)
- 安装 [node.js](http://nodejs.cn/) 
- 验证安装,具体的安装步骤就省略了[身为`猿类`这肯定都不是问题啦]
- 其中,若首次安装git, git 要和github关联,需要生成SSH-KEY并添加到github,这里也不赘述了！

#### 安装 hexo 

- 在`Git bash`窗口

``` bash
$ npm install hexo-cli -g

```

- 任意磁盘，新建文件夹 `eg:` `D:\HEXO`
- 在`Git bash` 或 `win+R  cmd`命令窗口，定位到`D:\HEXO`，然后依次执行
- `hexo`博客初始化

``` bash
$ hexo init
```

- 安装依赖

``` bash
$ npm install 
```

- 启动服务预览

```bash
$ hexo server
```

```bash
 INFO  Start processing
 INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```

`hexo` 本地搭建完毕，本地访问`http://localhost:4000/` 即可预览效果.

> 注：为了避免被`墙`，以上的`npm`镜像可以换成`淘宝npm`

```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

> 之后使用`npm`就可以换成`cnpm`.

## 关联部署到Github

在刚才生成博客的根目录下，找到`站点配置文件` `_config-yml` 修改 `# Deployment`部分

> 注意：`deploy` 每一项的参数都要留有 `空格`

``` bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/haoshuai6/haoshuai6.github.io.git
  branch: master
```

为了能够使Hexo部署到GitHub上，需要安装一个插件，避免出现 `ERROR Deployer not found : git`错误

``` bash
npm install hexo-deployer-git --save
```

到这里先 `STOP` 一会，

##### 思考一个问题： 

> 即当我重装电脑，或者在别的电脑，想要发布，修改文章时候，无法使用现在搭建好的hexo环境？

> 想法 ：用 `github` 两个分支，一个用来存放Hexo网站的源文件，一个用来发布网站(即hexo最后部署的静态文件)。

> 参考文章： http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more

---
###### 20161029 补充两个分支实现方法：
 
那么，目标是有两个分支：

> master分支：用来提交部署 `hexo` 在本地编译好的静态文件。即：博客的显示的静态文件

> source分支：用来备份、存储 `hexo` 在本地的原始程序文件。即：`hexo`博客的源文件

如何实现呢：

> 如果这么操作: 建好库之后，`clone` 到本地，然后在该文件夹下用hexo搭建，可以这样有一个问题：

> 当你`clone`到本地后，再进行`hexo init` 时，刚才`clone`的仓库版本控制信息会丢失，造成无法`push`

所以我是这么弄的 `[道行尚浅，还没想到好办法]`	

-  `clone` 库到本地一个空文件夹，那么文件夹名字一定是 `username`.github.io 
-  建立分支`source`，切换分支到`source`，将分支推送到远端，在`github` 修改默认分支为 `source` 
-  将刚才 `hexo` 本地生成博客的文件夹下的所有文件，都拷贝到，`username`.github.io 文件夹下[在分支`source`下操作]
-  或[`username`.github.io下的所有文件拷贝到，刚才 `hexo` 本地生成博客的文件夹下]，再用`github`客户端,`Add`本地库 

然后，执行下列指令即可完成部署：

``` bash
$ hexo generate
$ hexo deploy
```

之后，访问：`username`.github.io 浏览

---
###### 两个分支如何维护呢？

我在公司电脑，搭建的`hexo`博客，并且部署到了`github`，回到家里我想修改文章，家里电脑刚新装`git`和`node`

- 因为在github有了两个分支，默认分支为`source` 
- `clone`到本地，`hexo` 千万不要执行 **`hexo init`**  
-  直接 `hexo server` 有可能会`ERROR`提示  **`npm install hexo --save`** ，按照即可 `npm install`
-  然后就可以正常 `hexo server`啦，
-  然后就可以正常 编辑啦
-  然后就可以正常 `hexo clean` 和 `hexo g` 和  `hexo d` 啦

---
hexo常用命令(简写):

```bash
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo p == hexo publish #发布草稿
hexo g == hexo generate #生成静态网页
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署
```
更多命令：https://segmentfault.com/a/1190000002632530

---
`hexo` 支持 markdown格式，有空还得去学习下 markdown
