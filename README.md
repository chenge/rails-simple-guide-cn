Ruby请参考我写的简明Ruby系列：[Ruby简明入门和提高
](http://blog.csdn.net/freeagle/article/details/46659351)

(完善中......)

本指南版本：
* [github version](https://github.com/chenge/rails-simple-guide-cn/tree/master)
* [csdn version](http://blog.csdn.net/freeagle/article/details/46658607)（完整目录）


#前言

本指南定位为[rails官方指南](http://guides.ruby-china.org/)的导读，是为初学者写的，本书提供概要，细节请查阅官方指南。

##如何阅读

可以先尝试“一小时热身”，有兴趣的话可继续选学基础部分。

也许你关心需要多少时间学会，我给一个参考时间，请注意我是说的参考时间，入门一小时就够了，学习一个完整项目的话大概两个月吧，累计三个项目也就是六个月，可以达到中级水平吧。

另外，可以参考台湾网友xdite的内容丰富的免费入门课程[Rails 101](http://growth.xdite.net/courses/rails-101)。

#一小时热身
##安装的烦恼
本来rails的安装是很简单的，不过国内的网络环境让rails开发变得更困难了，目前似乎也没什么好的办法。

开始安装

假设ruby已经安装好了，基本的安装本来非常简单
```
gem install rails -V  #-V显示详细，可以不要
```

接下来生成一个简单的项目

```
rails new rails-rsg
```

接下来bundle，/Gemfile中列出了所有用到的gem，bundle就是一次安装好这些gem。

```
cd rails-rsg
bundle install -V  #-V显示详细，可以不要
```

顺利的话，会在数分钟内安装好吧。如果不行的话，可能要换淘宝的安装源，或者vpn。
##实验

这个热身实验的目的是提供一些感性认识先，大部分同学都不喜欢看理论的。

生成一个简单的博客的post。
```
rails g scaffold Blog title:string body:text
```
以上命令会生成一些建立数据表的程序，后续数据库部分会详细解释。

接下来，执行生成的建立数据表程序

```
rake db:migrate
```
顺利的话，数据表就建好了，默认是用sqlite3数据库，很简单实用。

最后一步，启动服务器。

```
rails s
```

浏览器就可以访问了。

```
http://localhost:3000/blogs
```
可以用浏览器操作下，有一些感性认识。

浏览器输入的网址是如何对应到程序的呢，这就是所谓的路由，下面看一下标准路由，如果没做过web开发的话，一开始可能看不太明白，有个印象就好了，Prefix就是路由名。

后面的路由部分会进一步学习。

![这里写图片描述](http://img.blog.csdn.net/20150627072819379)

#基础

##rails的设计原则
主要有两条：

* 不重复原则 DRY
* 惯例优于配置原则 CoC

不重复也就是简洁的意思吧，为不同的任务设计了DSL（领域特定语言）。
惯例原则可以减轻程序员的负担。

随着学习的深入，可以体会这两条原则。

##常用配置文件

```
/Gemfile 
/db/database.yml
/config/routes.rb
```
名字就能说明吧，就不详细解释了。热身中的路由就是用routes.rb生成的。

```
  resources :blogs
```
这一行就产生了7个标准路由，非常简洁，也就是rails被称为web DSL的原因吧。
##路由

参考热身实验的图，我以blog这个路由名为例，它是get方法，uri的写法是/blogs/1.html。 :id, :format是ruby的符号的写法。(.:format)表示可以省略这部分，最后对应控制器BlogsController的方法show。

基本路由是7个，update有两种，共计8个。

更复杂的有嵌套路由，还有member，collection路由。

##查错

```
raise @order.inspect
```
这个写法非常方便直观，浏览器就可以看，基本不用看终端的不太友好的黑屏幕了。

gem better_errors 可以改善默认的出错提示
gem byebug 可以设置断点


##数据库

这几条是最常用的数据库相关的rake命令。
```
rake db:drop:all
rake db:create
rake db:migrate
rake db:seeds
```
当需要修改数据表的时候，使用下面的命令，生成一个文件
```
rails g migration NameOfAny
```
在该文件中写上代码后（具体写法请查阅），然后运行就可以了。

```
rake db:migrate
```

mysql数据库需要gem mysql2

##rake

据说rails 5将不再用rake了。

如下命令列出所有task

```
rake -T
```

可以自己写task，task前加上desc "description"，就会出现在task列表中了。

##控制器
紧接着路由的就是控制器了。可以用命令生成：

```
rails g controller Samples
```

##模型

命令生成model

```
rails g model Sample title:string
```
这个会同时生成数据表迁移文件，请参考数据库部分


##部署
有两个选择，Capistrano和Mina，我用过前一种。据说后一种更简单些。
部署可以完成一些自动任务，比如数据库修改，这个是php用的ftp方式做不到的。

部署是相对复杂的话题，这里就不深入了。

##常用gem

[一篇不错的来自prograils的对gem的介绍，英文版](https://prograils.com/posts/ruby-gem-guide-how-to-install-and-work-with-local-gems)

##常用开发工具

*  pry

Chrome插件

*  postman
*  Mysql Admin

#传统的web开发
##视图

可以看看/app/views目录的文件，结合那个路由来理解。

#API开发
手机开发就不用视图了，反而简单了。
##JSON
简单的这样就可以了。
```
    render json: { blogs: @blogs }
```

##API文档

我目前就是用markdown写的，比较灵活，不过效果就一般。有一些流行的方案，目前还没用到。
