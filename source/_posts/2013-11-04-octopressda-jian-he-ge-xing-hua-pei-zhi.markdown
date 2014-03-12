---
layout: post
title: "octopress博客搭建和个性化配置"
date: 2013-11-04 15:02
comments: true
categories: octopress
tags: octopress
---
使用octopress搭建博客的人，大概都会喜欢写关于如何搭建配置octopress的文章。因为它的高定制性，为极客们带来很多乐趣。从首页的配置，到分类，评论，个人信息，社会化分享等的配置，内容繁多。而且这是我第二次搭建octopress环境了，发现上一次的配置有一部分不能clone到本机上。因此，把配置的过程记录下来是十分有必要的。  
我的意愿是记录配置的过程，顺便整理大牛们关于这方面的一些说明。网络上相关资料有很多，没有实践过的没有摘录在这里。所有的代码实现都是直接引用前辈们的。另外，本博的配置环境是linux fedora 18.  
<!-- more -->

####[octopress官方网站](http://octopress.org/)  
octopress的配置都可以在官网上找到。事实上，遇到问题查询官网文档是最有效率的方法。


###github的设置  
####创建仓库
登陆github网站，注册一个用户，假设为grunmin。  
创建一个仓库，命名为username.github.com，例如`grunmin.github.com`

####使用密钥登陆github
```
[[ -f ~/.ssh/id_rsa.pub ]] || ssh-keygen -t rsa     #生成密钥对
```
按默认一直确认即可。  
在github帐号设置里找到ssh的设置，添加一个ssh key。  
进入~/.ssh/找到id_rsa.pub文件，把里面的内容填到key里，title不填。  
这样做的好处是之后push到仓库上时可以不用输入密码。


###安装ruby
查看ruby版本  
```
ruby --version  # 必须显示1.9.3
```
安装方法：
```
curl -L https://get.rvm.io | bash -s stable --ruby
rvm install 1.9.3
rvm use 1.9.3
rvm rubygems latest
```
若有显示命名未找到，直接下载安装即可。


###安装octopress
在安装octopress之前，确保已安装git。
```
git clone git://github.com/imathis/octopress.git octopress
cd octopress
gem install bundler
bundle install
rake install
```
这里容易出现三个问题：  
1、已安装rvm和bundler，但显示找不到命令。

用绝对路径：`/home/username/.rvm/bin/rvm`
`/home/username/.rvm/bin/bundler`  
2、执行bundler install时显示：GemfileNotFound error?  

可能在安装的过程中退出了octopress目录，进入后执行即可。  
3、执行rake install时显示  
```
rake aborted!
No Rakefile found (looking for: rakefile, Rakefile, rakefile.rb, Rakefile.rb)
```
同理，进入安装目录即可。



###将博客部署到github上
```
rake setup_github_pages 
```
此时会要求输入仓库的url，可以在github仓库内容业的右下角找到。  
例如我的是` git@github.com:grunmin/grunmin.github.com.git`  
成功后即可用
```
rake new_post["title"]
```
生成新文章，文章在source/_post/目录下，文件名构成为时间和标题的拼音。之后执行  
```
rake generate
rake preview   
```
此时可以预览(浏览器打开localhost:4000可看到效果。）如果没有问题，则执行
```
rake deploy 
git add .
git comment -m "comment"
git push origin source 
```
保存到仓库。  
需要注意的是执行git命令时应处于octopress目录，并且checkout到source分支，
```
cd octopress
git checkout source
git add .
git commit -m "comment"
git push origin souce
```



###octopress博客的个性化配置

####添加文章分类（category)
1、增加category_list插件  
将下面的代码写到`plugins/category_list_tag.rb`
```
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```

2、增加侧边栏  
将下面的代码写到`source/_includes/asides/category_list.html`
```
<section>
  <h1>Categories</h1>
  <ul id="categories">
    {%raw%}{% category_list %}{%endraw%} 
  </ul>
</section>
```

修改_config.yml文件，找到`default_asides:`  
在中括号内添加
`asides/category_list.html`

添加的位置决定在页面显示的位置，注意用逗号隔开。  
用vim编辑器粘贴的话可能会自动缩进，启用粘贴模式或用其他编辑器即可。

####添加社会化评论
octopress 产生静态网页，不支持评论功能，所以我们用第三方评论系统。好消息是octopress已为我们配置好了Disqus，我们只需要稍微填写以下信息即可。  
1、首先在[Disqus](http://www.disqus.com/) 注册一个帐号，点击添加到我的网页，添加站点信息，比如我的`grunmin.github.io`  
2、修改_config.yml文件，找到这一段：
```
# Disqus Comments 
disqus_short_name: 
disqus_show_comment_count: false
```
添加你的disqus用户名，并把false修改成true即可。注意冒号后面有空格。  
此外，可以用国内的多说系统，速度较快，且比较符合景德镇村民的需要。之前我用的就是这个，但是没有记录配置过程，这次克隆时多说系统没有成功启动，因此不折腾了，有需要的话可以自行谷歌。
  
####导航栏设置
导航栏的设置在`source\_includes\custom\navigation.html`  
我们可以将Blog和Archives修改为首页和归档，也可以在此添加一个标签页，此时应使用命令`rake new_page['about']`创建一个页面，页面路径为`source\about\index.markdown`;  
修改后的文件如下：
```
<ul class="main-navigation"> 
  <li><a href="{{ root_url }}/">首页</a></li> 
  <li><a href="{{ root_url }}/blog/archives">归档</a></li> 
  <li><a href="{{ root_url }}/about">关于</a></li> 
</ul>
```

###从新电脑上恢复博客内容：
如果电脑重装了系统，或者要在另一台电脑编写博客，可以这样设置。
找到博客仓库的url，执行
```
git clone -b source (url) octopress   #把source 克隆到本地octopress目录上
```
```
cd octopress
git clone (url) _deploy   #克隆master分支，它存放着博客内容。
```
```
gem install bundler
bundle install
rake install
rake setup_github_pages
```
之后操作同上，有问题照葫芦画瓢。


###其他问题
如果不能生成或者推送博客了，可以采取下述措施：

  - 在命令行输入`/bin/bash --login`，重试
  - 查看ruby的版本，如果不是1.9.3的话，执行`rvm use 1.9.3`


  
**持续更新…………**


####参考文章：
[利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)

[Ruby开源项目介绍(1)：octopress——像黑客一样写博客](http://www.yangzhiping.com/tech/octopress.html)


[为octopress添加分类(category)列表](http://codemacro.com/2012/07/18/add-category-list-to-octopress/)


[冯威的学习专栏---记录工作学习点滴](http://www.cnblogs.com/oec2003/archive/2013/05/31/3109577.html)


