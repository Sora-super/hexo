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

// pwd可以获取当当前路径
```

### 更多资源

1. [vuepress搭建文档网站](http://doc.xuezsl.com/)

2. [仿简书文章网站](http://www.xuezsl.com/)

3. [博客网站](http://hexo.xuezsl.com/)
