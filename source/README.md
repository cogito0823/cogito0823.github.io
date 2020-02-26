# cogito0823.github.io
## 概述
**这是一个基于 hexo 的个人博客项目，hexo 分支为默认分支，用以保存博客源文件，master 分支用以托管博客页面**

**目录：**
- 如何克隆此项目
- 主题
- 主题配置日志
- 博文发布日志
- 参考
## 如何克隆此项目
如前概述所述，hexo 分支保存博客源文件，如需克隆完整博客项目只需获取 hexo 分支里的源文件。具体操作如下：
- fork 本项目的 hexo 分支到你的 github 账户下
- 新建一个仓库，命名为：username.github.io，其中 username 为你的 github 账户名，下文出现的 username 亦然
- 安装 git
```
sudo apt-get install git
```
- 设置git全局邮箱和用户名
```
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```
- 安装nodejs
```
sudo apt-get install nodejs
sudo apt-get install npm
```
- 安装hexo
```
sudo npm install hexo-cli -g
```
- 选择一个存放项目的合适位置，cd 进该目录,克隆你的项目
```
git clong ‘你的项目的 clong 链接（一般是 https://github.com/username/username.github.io.git 或 git@github.com:username/username.github.io.git）’
```
- 进入克隆到的文件夹安装 hexo 配置 git 到远端的插件：
```
npm install
npm install hexo-deployer-git --save
```
- 修改配置文件‘_config.yml’
```
vi _config.yml
```
在 _config.yml 里找到 
```
deploy:
  type: 
  repository:
  branch:
```
将“repository: ”后面的链接改成你的 username.github.io 仓库的克隆链接，一般是：https://github.com/username/username.github.io.git 或 git@github.com:username/username.github.io.git，
注意冒号和链接之间有空格，修改完后保存并推出
- 生成静态页面：
```
hexo clean
hexo g
```
- 在本机 4000 端口调试
```
hexo s
```
打开浏览器输入 localhost:4000 敲击回车可以看到生成的静态页面
- 部署
```
hexo d
```
浏览器输入 username.github.io 敲击回车看到部署到 github 的页面

***！！！注意：将上文所有 username 改为你的 GitHub 用户名***
## 主题
- [NexT](https://github.com/theme-next/hexo-theme-next)
- [Yilia](https://github.com/litten/hexo-theme-yilia)
## 主题配置日志
- 2019
  - 07-05 
    - 在右上角或者左上角实现fork me on github
    - 修改作者
    - 添加 busuanzi 访问量
    - 设置网站的图标
    - 隐藏网页底部 powered By Hexo / 强力驱动
    - DaoVoice 在线联系
## 博文发布日志
- 2019
  - 07-05 [使用PicGo搭建github图床](https://blog.cogito0823.github.io/2019/07/05/%E4%BD%BF%E7%94%A8PicGo%E6%90%AD%E5%BB%BAgithub%E5%9B%BE%E5%BA%8A/)
  - 07-04 [使用hexo+github搭建个人博客](https://blog.cogito0823.github.io/2019/07/05/%E4%BD%BF%E7%94%A8hexo-github%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
## 参考
- [reuixiy | 打造个性超赞博客 Hexo + NexT + GitHub Pages 的超深度优化](https://io-oi.me/tech/hexo-next-optimization.html)
- [Moorez | hexo的next主题个性化教程:打造炫酷网站](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
- [cogito | 使用hexo+github搭建个人博客](https://blog.ccogito.xyz/2019/07/04/%e4%bd%bf%e7%94%a8hexogithub%e6%90%ad%e5%bb%ba%e4%b8%aa%e4%ba%ba%e5%8d%9a%e5%ae%a2/)
- [cogito0823.github.io](https://blog.cogito0823.github.io)
