如何在Linux中以交互方式分析和查看Apache web服务器日志?
================================================================================

无论你是在网站托管业务，还是在自己的VPS上运行几个网站，你总会有机会想要显示访客数量例如前几的房客，请求使用的文件（无论是动态或者是静态），带宽的使用，客户端的浏览器，和相关的网站，等等。


[GoAccess][1] 是一款用于Apache或者Nginx命令行日志分析和交互式查看器。有了这款工具，你不仅可以浏览到之前提及的相关数据，还可以分析网站服务器日志来进一步挖掘数据 - 然而 **这一切都可以在一个终端窗口实时输出**.由于今天的[大多数web服务器][2]使用一个Debian的衍生版或者基于红帽发行版来作为底层操作系统，我将会告诉你如何在Debian和CentOS中安装和使用GoAccess。


### 在Linux系统安装GoAccess ###


在Debian，Ubuntu及其衍生版本，运行一下命令来安装GoAccess：

    # aptitude install goaccess 

在CentOS中，你将需要使你的[EPEL 仓库][3]可用然后执行以下命令：

    # yum install goaccess

在Fedora，同样使用yum命令：

    # yum install goaccess 


如果你想从源码安装GoAccess来使后续的功能可用（例如 GeoIP 的位置），为你的操作系统安装[必需的依赖包][4]，按以下步骤进行：

    # wget http://tar.goaccess.io/goaccess-0.8.5.tar.gz   
    # tar -xzvf goaccess-0.8.5.tar.gz
    # cd goaccess-0.8.5/
    # ./configure --enable-geoip
    # make
    # make install 


以上安装的版本是 0.8.5，但是你也可以在该软件的网站[下载页][5]确认是否是最新版本。


由于GoAccess不需要后续的配置，一旦安装你就可以马上使用。


### 运行 GoAccess ###

开始使用GoAccess，只需要对它运行你的Apache访问日志。


对于Debian及其衍生版本：

    # goaccess -f /var/log/apache2/access.log


基于红帽的发型版本：

    # goaccess -f /var/log/httpd/access_log 


当你第一次启动GoAccess，你将会看到下方屏幕中选择日期和日志格式。正如前面所述，你可以选择在空格键和F10之间相互切换。至于日期和日志格式，你可能希望参考[Apache 文档][6]来刷新你的记忆。


在这个例子中，选择常见日志格式（CLI）：

![](https://farm8.staticflickr.com/7422/15868350373_30c16d7c30.jpg)

然后按F10.你将会从屏幕中获得统计数据。为了简约，只显示首部，也就是总结日志文件的摘要，如下图所示：


![](https://farm9.staticflickr.com/8683/16486742901_7a35b5df69_b.jpg)

### 通过 GoAccess来浏览网站服务器统计数据 ###

当你通过向下的剪头滚动页面，你会发现一下章节，按要求进行排序。这里提及的目录顺序可能会根据你的发型版本或者（从源和库）首选的安装方式：

1. 每天唯一访客（具有同样IP，同一日期和统一代理被认为是）

![](https://farm8.staticflickr.com/7308/16488483965_a439dbc5e2_b.jpg)

2. 请求的文件（网页URL）


![](https://farm9.staticflickr.com/8651/16488483975_66d05dce51_b.jpg)

3. 请求的静态文件（例如，.png文件，.js文件等等）

4. 请求的URLs（每一个URL请求的出处）

5. HTTP 404 不能找到响应的代码

![](https://farm9.staticflickr.com/8669/16486742951_436539b0da_b.jpg)

6. 操作系统

7. 浏览器

8. 主机（客户端IP地址）

![](https://farm8.staticflickr.com/7392/16488483995_56e706d77c_z.jpg)

9. HTTP 状态代码

![](https://farm8.staticflickr.com/7282/16462493896_77b856f670_b.jpg)

10. 前几位的推荐站点

11. 在谷歌的搜索引擎使用的排名在前的关键字


如果你还想检查已经存档的日志，你可以在GoAccess通过使用管道符号如下。

在Debian及其衍生版本：

    # zcat -f /var/log/apache2/access.log* | goaccess 

在基于红帽的发型版本：

    # cat /var/log/httpd/access* | goaccess 


如果你需要任何更多关于以上的详细报告（1至11项），直接按下章节序号再按O（大写o），就可以显示出你需要的详细视图。下面的图像显示5-O的输出（先按5，再按O）

![](https://farm8.staticflickr.com/7382/16302213429_48d9233f40_b.jpg)


如果要现实GeoIP位置信息，打开详细视图的主机部分，如前面所述，你将会看到客户端IP地址所在的位置以及显示web服务器的请求。


![](https://farm8.staticflickr.com/7393/16488484075_d778aa91a2_z.jpg)


如果你的系统还尚未达到很忙碌的状态，以上提及的章节将不会显示大量的信息，但是这种情形可以通过在你网站服务器越来越多的请求发生改变。

###  在线保存分析的报告 ＃＃＃


当然有时候你不想每次都实时去检查你的系统状态，但是保存一份在线的分析文件或者打印版是由必要的。要生成一个HTML报告，只需要通过之前提到GoAccess命令输出来简单地重定向道一个HTML文件。然后，你只需通过web浏览器来将这份报告打开即可。



    # zcat -f /var/log/apache2/access.log* | goaccess > /var/www/webserverstats.html


一旦报告生成，你将需要点击展开的链接来显示每个类别详细的视图信息：

![](https://farm9.staticflickr.com/8658/16486743041_bd8a80794d_o.png)

注释：youtube视频
<iframe width="615" height="346" frameborder="0" allowfullscreen="" src="https://www.youtube.com/embed/UVbLuaOpYdg?feature=oembed"></iframe>


正如我们通过这篇文章讨论，GoAccess是一个非常可贵的工具，它提供给作为百忙之中的系统管理员一份HTTP统计的静态可是报告。虽然GoAccess默认其输出结果为标准输出，但是你也可以将他们保存到JSON，HTML或者CSV文件。这样的转换，GoAccess将作为一个非常有用的工具来监控和显示网站服务器的统计数据。

--------------------------------------------------------------------------------

via: http://xmodulo.com/interactive-apache-web-server-log-analyzer-linux.html

作者：[Gabriel Cánepa][a]
译者：[disylee](https://github.com/译者ID)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创翻译，[Linux中国](http://linux.cn/) 荣誉推出

[a]:http://xmodulo.com/author/gabriel
[1]:http://goaccess.io/
[2]:http://w3techs.com/technologies/details/os-linux/all/all
[3]:http://xmodulo.com/how-to-set-up-epel-repository-on-centos.html
[4]:http://goaccess.io/download#dependencies
[5]:http://goaccess.io/download
[6]:http://httpd.apache.org/docs/2.4/logs.html
