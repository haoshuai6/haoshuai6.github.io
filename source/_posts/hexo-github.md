---
title: Hexo+Github Pages搭建个人博客[两个分支]

---
一个高逼格的博客-- `Hexo + github` ,这个组合免费、静态化、部署方便！编写文章使用Markdown，这可真是，很吸引人啊！

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

然后，执行下列指令即可完成部署：

``` bash
$ hexo generate
$ hexo deploy
```

之后，访问：`username`.github.io 浏览

---

hexo常用命令(简写):

```bash
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo p == hexo publish
hexo g == hexo generate#生成静态网页
hexo s == hexo server #启动服务预览
hexo d == hexo deploy#部署
```
更多命令：https://segmentfault.com/a/1190000002632530

##### 问题： 

> 建好库之后，`clone` 到本地，然后用hexo搭建，可以这样有一个问题：
当你`clone`到本地后，再进行`hexo init` 时，刚才`clone`的仓库版本控制信息会丢失，造成无法`push`

> 即当我重装电脑，或者在别的电脑，发布，修改文章时候，无法使用现在搭建好的heox环境

想法 ：有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

参考文章： http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more
