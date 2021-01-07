# hexo
hexo个人博客

## 本地开发
npm run server

## 打包
npm run build

## 发布

可以采用FTP、git仓库等方式将打包好的publc文件上传到服务器

下面是使用scp命令

```
scp -r 本地文件路径 root@服务器ip:服务器此文件地址

例:
scp -r /Users/xuewangyi/hexo/person/public root@123.56.244.137:/www/admin/hexo.xuezsl.com_80/wwwroot

// pwd可以获取当前路径
```

### 更多资源

1. [vuepress搭建文档网站](http://doc.xuezsl.com/)

2. [仿简书文章网站](http://www.xuezsl.com/)

3. [博客网站](http://hexo.xuezsl.com/)


### 生成项目结构树

``` bash
tree -l 2, -o tree.md --ignore "node_modules/"
```

### application

[本项目地址](https://github.com/settings/applications/1447277)

### 目录结构
```
├── README.md
├── _config.landscape.yml
├── _config.yml
├── db.json
├── package-lock.json
├── package.json
├── public
|  ├── 2020
|  |  └── 12
|  |     └── 31
|  |        ├── hello-world
|  |        |  └── index.html
|  |        └── 创建博客
|  |           └── index.html
|  ├── 2021
|  |  └── 01
|  |     ├── 04
|  |     |  └── createPersistedState-vuex-持久化缓存
|  |     |     └── index.html
|  |     └── 05
|  |        └── sequelize简单操作
|  |           └── index.html
|  ├── about
|  |  └── index.html
|  ├── archives
|  |  ├── 2020
|  |  |  ├── 12
|  |  |  |  └── index.html
|  |  |  └── index.html
|  |  ├── 2021
|  |  |  ├── 01
|  |  |  |  └── index.html
|  |  |  └── index.html
|  |  └── index.html
|  ├── categories
|  |  └── index.html
|  ├── css
|  |  └── diaspora.css
|  ├── img
|  |  ├── cover.jpg
|  |  ├── favicon.png
|  |  ├── logo.png
|  |  └── welcome-cover.jpg
|  ├── index.html
|  ├── js
|  |  ├── diaspora.js
|  |  ├── jquery.min.js
|  |  ├── plugin.js
|  |  └── typed.js
|  ├── photoswipe
|  |  ├── default-skin
|  |  |  ├── default-skin.css
|  |  |  ├── default-skin.png
|  |  |  ├── default-skin.svg
|  |  |  └── preloader.gif
|  |  ├── photoswipe-ui-default.js
|  |  ├── photoswipe-ui-default.min.js
|  |  ├── photoswipe.css
|  |  ├── photoswipe.js
|  |  └── photoswipe.min.js
|  ├── search
|  |  └── index.html
|  ├── search.xml
|  └── tags
|     └── index.html
├── scaffolds
|  ├── draft.md
|  ├── page.md
|  └── post.md
├── source
|  ├── _posts
|  |  ├── createPersistedState-vuex-持久化缓存.md
|  |  ├── hello-world.md
|  |  ├── sequelize简单操作.md
|  |  └── 创建博客.md
|  ├── about
|  |  └── index.md
|  ├── categories
|  |  └── index.md
|  ├── search
|  |  └── index.md
|  └── tags
|     └── index.md
└── themes
   └── diaspora
      ├── LICENSE
      ├── README.md
      ├── _config.yml
      ├── languages
      |  ├── default.yml
      |  ├── fr.yml
      |  ├── it.yml
      |  ├── nl.yml
      |  ├── no.yml
      |  ├── ru.yml
      |  ├── zh-CN.yml
      |  └── zh-TW.yml
      ├── layout
      |  ├── _partial
      |  |  ├── categories.ejs
      |  |  ├── google-analytics.ejs
      |  |  ├── head.ejs
      |  |  ├── list.ejs
      |  |  ├── mathjax.ejs
      |  |  ├── menu.ejs
      |  |  ├── pagination.ejs
      |  |  ├── photoswipe.ejs
      |  |  ├── post
      |  |  |  ├── article.ejs
      |  |  |  ├── date.ejs
      |  |  |  ├── gitalk.ejs
      |  |  |  ├── header.ejs
      |  |  |  ├── item.ejs
      |  |  |  ├── tag.ejs
      |  |  |  └── title.ejs
      |  |  ├── screen.ejs
      |  |  ├── scripts.ejs
      |  |  ├── search.ejs
      |  |  └── tags.ejs
      |  ├── archive.ejs
      |  ├── category.ejs
      |  ├── index.ejs
      |  ├── layout.ejs
      |  ├── page.ejs
      |  ├── post.ejs
      |  └── tag.ejs
      ├── scripts
      |  └── page_title.js
      └── source
         ├── css
         |  └── diaspora.css
         ├── img
         |  ├── cover.jpg
         |  ├── favicon.png
         |  ├── logo.png
         |  └── welcome-cover.jpg
         ├── js
         |  ├── diaspora.js
         |  ├── jquery.min.js
         |  ├── plugin.js
         |  └── typed.js
         └── photoswipe
            ├── default-skin
            |  ├── default-skin.css
            |  ├── default-skin.png
            |  ├── default-skin.svg
            |  └── preloader.gif
            ├── photoswipe-ui-default.js
            ├── photoswipe-ui-default.min.js
            ├── photoswipe.css
            ├── photoswipe.js
            └── photoswipe.min.js
```

