# 信息搜集

## web1
右键查看源码，得到flag
## web2
提示无法查看源码，快捷键直接ctrl+u就能看到源码了，注释里面就是flag。
或者url前加入view-source: 查看源代码。
## web3
burpsuite抓包，发包后查看返回包内容。发现flag
## web4
robots.txt 一个非常重要得到文件，通常情况下，主要用于指定搜索引擎蜘蛛spider在网站里的抓取范围,用于声明蜘蛛不可以抓取哪些网站资源及可以抓取哪些网站资源。
访问robots.txt文件，遂发现disallow路径，然后访问路径以后找到flag
## web5
本题考查的是phps作为备份文件，泄露了源码。访问/index.phps下载文件拿到flag
其他常见的有linux的备份文件，比如index.php.swp
还有www.zip等。
注意后缀可以结合当前文件，比如index.php来修改，如果是upload.php则可以改为upload.php.zip等。

## web6
www源码泄露
访问url/www.zip，通过其源码泄露，发现flag

## web7
.git信息泄露

根据提示版本控制，想到常用的版本控制工具git，svn，尝试访问.git和.svn，在.git中发现flag

## web8
SVN信息泄露 /.svn/ 路径拼接 .svn SVN（subversion）源代码版本管理软件，造成SVN源代码漏洞的主要原因是管理员操作不规范。在使用SVN管理本地代码过程中，会自动生成一个名为.svn的隐藏文件夹，包含重要的源代码信息,开发管理员在发布代码时，不愿意使用‘导出’功能，而是直接复制代码文件夹到WEB服务器上，导致.svn隐藏文件夹被暴露于外网环境

.svn缓存信息泄露，直接访问url/.svn/

## web9
vim缓存泄露，在使用vim进行编辑时，会产生缓存文件，操作正常，则会删除缓存文件，如果意外退出，缓存文件保留下来，这是时可以通过缓存文件来得到原文件，以index.php来说，第一次退出后，缓存文件名为 .index.php.swp，第二次退出后，缓存文件名为.index.php.swo,第三次退出后文件名为.index.php.swn

```bash
vim - r .index.php.swp
```
可以还原文件。

另外一个点，在linux中，用gedit编辑器保存后，当前目录下会生成一个后缀为“\~”的文件，其文件内容就是刚编辑的内容，假如才保存的文件为“flag”，那么该文件名为“flag~”。

## web10
cookie信息泄露

直接想办法查看cookie内容
