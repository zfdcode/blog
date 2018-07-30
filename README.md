# Blog
这是我基于Hexo的静态博客源文件。使用了next主题并使用了travis进行自动部署。[博客地址](http://blog.zfdcode.com)  
欢迎fork后自行使用。(不需要在本地端安装hexo)

# 使用步骤
- 在本地pull代码并删除我个人的posts
- 修改_config.yml内#Site、#URL下个人信息
- 修改.travis.yml内git参数
- 将git项目关联[Travis CI](https://travis-ci.org/)并将项目token储存在Travis设置中，参见[使用 Travis CI 自动更新 GitHub Pages](http://notes.iissnan.com/2016/publishing-github-pages-with-travis-ci/)

- 参考Travis CI设置文档：
```json
{
  "os": "linux",
  "dist": "trusty",
  "group": "stable",
  "script": [
    "hexo g"
  ],
  "install": [
    "npm install"
  ],
  "node_js": "stable",
  "language": "node_js",
  "global_env": "GH_REF=github.com/zfdcode/zfdcode.github.io.git",
  "after_script": [
    "cd ./public",
    "git init",
    "git config user.name \"zfdcode\"",
    "git config user.email \"zfdgame@outlook.com\"",
    "git add .",
    "git commit -m \"Update docs\"",
    "git push --force --quiet \"https://${GH_TOKEN}@${GH_REF}\" master:master"
  ],
  "before_script": [
    "npm install -g hexo-cli",
    "npm install hexo-footnotes --save"
  ]
}
```
该设置会自动安装npm并通过npm安装hexo，之后从指定仓库抓取hexo博客源文件并生成静态网页文件，最后将生成的文件推送到指定仓库。
