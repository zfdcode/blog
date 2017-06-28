# Blog
这是我基于Hexo的静态博客源文件。使用了next主题并使用了travis进行自动部署。[博客地址](http://blog.zfdcode.com)  
欢迎fork后自行使用。(不需要在本地端安装hexo)

# 使用步骤
- 在本地pull代码并删除我个人的posts
- 修改_config.yml内#Site、#URL下个人信息
- 修改.travis.yml内git参数
- 将git项目关联[Travis CI](https://travis-ci.org/)并将项目token储存在Travis设置中，参见[使用 Travis CI 自动更新 GitHub Pages](http://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)
