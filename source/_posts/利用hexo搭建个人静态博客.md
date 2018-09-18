---
title: 利用hexo搭建个人静态博客
date: 2018-09-16 21:49:42
urlName: User_Pages_by_Hexo
tags: 
    - hexo
    - User Pages
    - SEO
---

> 之前一直想搭一个个人博客, 由于各种原因搁置了. 这次无意中看到`github pages`的妙用, 趁着热情还没散去赶紧动手搭建了一个, 作为一个后端菜鸡程序员, 折腾这么一个前端也是踩了不少坑. 包括安装部署以及一些个性化的配置, 这里记录一下, 作为`Hello World`. 

<!-- more -->

## 安装环境

  > 说到安装环境就要在这里啰嗦下, 开始是用的`github`推荐的`jekyll`, 还要安装`ruby`, 找到`next`主题的过程中了解到了`hexo`, 果断换了, 这个不会还可以找个队友问下. 就是这么一个过程就导致了我误解了这个流程. 由于`github`上`next`的文档有点歧义, 导致我直接下载主题之后在主题的根目录下运行`hexo server`.由于第一次接触`node`, 这里就直接怀疑人生了, 由于文是框架快完工的时候写的, 所以有些坑可能就忘了.  

  1. 安装`node`. 

  2. 在命令行执行``npm install -g hexo-cli`` 安装`Hexo`
   
  3. 执行``hexo init`` 初始化项目

  4. 在项目根目录执行``npm install`` 安装项目依赖

     > 到这里环境算是搭建好了, 可以执行``hexo s -g`` 然后去浏览器访问``localhost:4000``查看效果. 具体的可以查看[Hexo官网](https://hexo.io/zh-cn/docs/)

  5. 更换主题, 你可以把主题下载到你项目的`themes`目录下面, 然后修改项目根目录下面的配置文件`_config.yml`中的`theme`字段为 你下载下来的主题文件夹名, 注意`theme`后面必须要有空格. 我装的是[Next](https://github.com/iissnan/hexo-theme-next)


## `hexo` 部署到`github` 和`coding`

> 博客我原本是放在`github`上, 但是由于访问速度比较慢, 还有百度并不能收录到`github` 的内容, 然后就把`coding` 作为国内访问, 毕竟配置好了之后更新文章和只放`github`没区别. 

### 配置`ssh`公钥

> balabala

###  `hexo` 部署到`gibhub`, `master`分支存博客, `source`分支存源码 

> 利用分支同时保留博客静态文件和源代码, 由于我会在公司和家里写博客, 所以方便在不同电脑之间同步就很重要了. 可以把代码放在`source`分支, 这样没有写完的文章等可以直接传上去, 静态博客自动部署在`master`分支, 这样就两不耽误. 需要注意的是, 不要手动修改`master`分支的内容, 否则下次部署又重新覆盖掉了.

1. 新建一个 `gubhub`仓库, 项目名 **一定要叫`UserName.github.io`** 其中`UserName`为你的`github`用户名.

    > 敲重点: 开始我开到别人说我也不信, 页面也能访问到也就没在意, 一抓包发现发现怎么也加载不出`css`等文件, 域名也是`UserName.github.io`, 应该是域名和仓库名需要对应. 这种格式的仓库为`User Pages`, 和普通仓库还有一个不同的地方就是这种仓库的`pages`只能放在`master`分支, 普通仓库的`pages`可以一般可以放在`gh-pages`分支. 这样就导致了一个很烦人的问题, 进入仓库的时候主分支没有`readme`, 他会很显眼的提示你加上, 你加上之后又会给你渲染成很丑的样子, 要么就是每次生成都要重新手动加入`public`文件夹里面, 这是要逼死强迫症啊. 还好[<span id = "skip_render_back">解决了~~</span>](#skip_render)

2. 安装`git`

3. 在项目根目录执行`git init`初始化本地`git`仓库

4. 编辑`.gitignore`文件, 避免把一些依赖和代码自动生成的文件传到`github`上, 我的文件如下
    ```
    .DS_Store
    Thumbs.db
    db.json
    package-lock.json
    *.log
    node_modules/
    public/
    .deploy*/

    .git/
    .idea/
    ```

6. 到项目根目录分别执行以下命令: (git不熟可以看看[git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000))
    ```
    # 创建并切换分支
    git checkout -b source
    git add .
    git commit -m "first commit"
    git remote add origin git@github.com:blueboxH/blueboxh.github.io.git
    git push -u origin source
    ```
    > 把其中的`blueboxH`换成你的用户名, 到这里你就把源码推送到了服务器的`source`分支, `master`分支不用手动推送, 最后要是推送不上去按照提示操作强制推送分支就好了. 不能推送的话检查`ssh`公钥配置是否有误

6. 在项目根目录`_config.yml`中`deploy`字段配置如下:
    ```
    deploy:
      type: git
      repo: git@github.com:blueboxH/blueboxh.github.io.git
      branch: master
    ```
7. 执行`hexo g -d` 将自动生成后的文件自动部署到`master`分支. 

8. 去你的仓库设置检查`github pages`配置. 按以上步骤操作的话应该会自动设置好. 可以点击上面显示的链接访问你的站点了. 

### `hexo` 部署到`coding`

> 由于源代码已经在`github`仓库了, 所以没必要再放到`coding`, 直接部署静态博客到`coding`上就好了.

  1. 注册`coding`账号, 升级成为银牌会员. 创建名为`username`的仓库, 仓库名字和用户名相同. (不同怎么样我也没试过~~) 仓库需要设置为公开源代码(新版). 

  2. 配置项目根目录下`_config.yml`文件, 如下: (我的用户名为`blueboxH`, 这个根据个人情况修改)
      ```yaml
      deploy:
        type: git
        repo: 
          github: git@github.com:blueboxH/blueboxh.github.io.git
          coding: git@git.coding.net:blueboxH/blueboxH.git
        branch: master
      ```
  3. 执行`hexo g -d` 将自动生成后的文件自动部署到`github`和`coding`的`master`分支. 

        > 要是这里部署不成功的话(`master`分支没有文件), 请检查`ssh`公钥配置. 

  4. 在 `代码` > `pages服务`中点击设置`一键开启静态pages`

### 绑定域名
> 之所以把绑定域名单独提出来, 是因为这里有坑. 还有妙用, 如果你只是把项目放在一个仓库里, 那绑定域名只是一个可有可无的操作, 但是当你把博客同时部署在`github`和`coding`上的时候, 这能给你带来很多方便. 比如, 百度和谷歌收录不用再根据域名单独配置了, 同一个域名就可以访问部署在两个地方的博客. 域名解析可以把同一个域名根据访问`ip`不同分别解析到`github`或`coding`上, 只需要把解析到`github`的线路设置为海外即可, 你也可以把`www`前缀的域名解析到`coding`上(`github`不可以解析多个域名), 毕竟还是有一些人错误的认为域名必须要加`www`, 虽然猿类犯这种错的几率不大~~

[参考链接](http://zhanghao.studio/%E6%8F%90%E5%8D%87%E5%8D%9A%E5%AE%A2%E7%9A%84%E8%AE%BF%E9%97%AE%E9%80%9F%E5%BA%A6.html)

这个很简单, 就如上文所操作, 只不过有两个地方需要注意,

-  一个就是公钥不需要两个, 邮箱相同, 同一个公钥可以在两个仓库设置.
- 去掉`coding`广告的时候需要回到旧版才能看到`Hosted by Coding Pages`选项

## `hexo`有层次的分类(TOC)

## `hexo` SEO

> 你的博客部署上去了, 但是此时你并不能通过`google`或者`百度`搜索到你的博客, 因为你没有去他们那里登记~~. 你**首先**得向他们证明这个博客是你的, 这个时候就能体会到分别部署在两个地方的时候用属于自己的独立域名的好处了, 你可以用域名解析的方式方便的证明这个网站是你的, 不然两个部署在两个地方用两个不同的域名, 操作起来很麻烦.  **然后**你再分别提交站点地图.  这里我就没什么好说的了, 并没有什么独特的见解, 网上教程大把. 不过还是期待以后有时间能丰富这个模块. 所以单独留出来占个坑

1. 安装sitemap站点地图自动生成插件

   ```shell
   npm install hexo-generator-sitemap --save
   npm install hexo-generator-baidu-sitemap --save
   ```

2. 在项目根目录`_config.yml`下作如下配置:

   ```yaml
   sitemap:
     path: resources/sitemap.xml
   baidusitemap:
     path: resources/baidusitemap.xml
   ```

   > 之所以不配置在主题目录是因为配置在这换主题的话不用改, 且这个和主题无关

3. 在项目根目录`_config.yml`下修改域名:

   ```yaml
   # URL
   ## 如果你的站点用的子目录(比子域名更有利于SEO), 设置 url 为 'https://bluebox.org.cn/blog' ,设置 root 为 '/blog/'
   url: https://bluebox.org.cn
   root: /
   ```

4. 执行`hexo g`生成静态文件, 你会发现 public/resource下面多了两个文件`sitemap.xml`和baidusitemap.xml`

5. 添加`robot`协议, 在``source\resources``下面新建`robot.txt`文件, 内容如下:

   ```txt
   # hexo robots.txt
   User-agent: *
   Allow: /
   Allow: /archives/
   
   Disallow: /vendors/
   Disallow: /js/
   Disallow: /css/
   Disallow: /fonts/
   Disallow: /vendors/
   Disallow: /fancybox/
   
   Sitemap: https://bluebox.org.cn/resources/sitemap.xml
   Sitemap: https://bluebox.org.cn/resources/baidusitemap.xml
   
   ```

   6. [给非友情链接的出站链接添加 "nofollow" 标签](https://www.arao.me/2015/hexo-next-theme-optimize-seo/#u7ED9_u975E_u53CB_u60C5_u94FE_u63A5_u7684_u51FA_u7AD9_u94FE_u63A5_u6DFB_u52A0__u201Cnofollow_u201D__u6807_u7B7E)

## `hexo`填坑

### [<span id = "skip_render">`hexo`不渲染`README`文件的方法</span>](#skip_render_back "back")   

  在项目根目录下`_config.yml`文件中配置如下配置:
  ```
  skip_render: README.md
  ```
## todo
- 怎么默认 source 分支
- readme
- 代码复制粘贴插件
- 1, 2级标题居中显示



