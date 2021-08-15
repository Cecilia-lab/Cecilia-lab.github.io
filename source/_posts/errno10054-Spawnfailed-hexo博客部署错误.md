---
title: errno10054-Spawnfailed-hexo博客部署错误
date: 2021-08-15 22:14:29
comments: false
categories: 笔记
tags: hexo
---
# 错误
今天在部署博客的时候遇到错误：
```sh
fatal: unable to access 'https://github.com/Cecilia-lab/Cecilia-lab.github.io/': OpenSSL SSL_read: Connection was reset, errno 10054
FATAL {
  err: Error: Spawn failed
      at ChildProcess.<anonymous> (E:\code\Github\Cecilia-lab.github.io\node_modules\hexo-util\lib\spawn.js:51:21)
      at ChildProcess.emit (events.js:400:28)
      at ChildProcess.cp.emit (E:\code\Github\Cecilia-lab.github.io\node_modules\cross-spawn\lib\enoent.js:34:29)
      at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12) {
    code: 128
  }
} Something's wrong. Maybe you can find the solution here: %s https://hexo.io/docs/troubleshooting.html
```
在网上查了一下发现有两种主要方法。

# 方法一
进入站点根目录后，在git bash中执行：
```sh
#删除.deploy_git文件夹
rm -rf .deploy_git/

#windows中的换行符为 CRLF，而在Linux下的换行符为LF，这里将自动转换禁用
git config --global core.autocrlf false

#清空原来的静态文件
hexo clean #缩写为 hexo cl

#生成新的静态文件
hexo generate #缩写为 hexo g

#部署
hexo deploy #缩写为 hexo d
```
# 方法二：https协议的SSL模式导致的问题

在git bash中执行：
```sh
#设置git的SSL协议为false
git config --global http.sslVerify "false"
```
在cmd中执行：
```sh 
ipconfig /flushdns
```
刷新DNS。

>此方法来自CSDN博主「Kaysa_8023」
————————————————
版权声明：本文为CSDN博主「Kaysa_8023」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhangxiaozhe8023/article/details/119166897


# 方法三：git repo配置地址不正确,可以将http方式变更为ssh方式
进入站点根目录后，在git bash中执行：
```sh
#编辑_config.yml文件
vim _config.yml
```
在_config.yml文件中找到这一段：
```sh
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io.git
  branch: master
```
并改为：
```sh
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
```
可以在github的code-clone的SSH选项中复制自己的地址。

然后重新生成并部署。
```sh
#清空原来的静态文件
hexo clean #缩写为 hexo cl

#生成新的静态文件
hexo generate #缩写为 hexo g

#部署
hexo deploy #缩写为 hexo d
```
>此方法和方法一来自CSDN博主「wei-xiansen」
————————————————
版权声明：本文为CSDN博主「wei-xiansen」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_41256398/article/details/117994899

# 方法四：网络问题，多试几次

最后方法三解决了我的问题，原来是忘记改配置地址了😓