---
title: 用hexo在github上搭建博客
date: 2018-03-14
---

> 我对git很熟悉，hexo很陌生。如果你和我一样，这篇会很有帮助。我会对操作进行简单说明，你可以先通读全文，再实际操作。最后部分讲博客的版本管理，实现不同电脑上的更新和维护。

# 环境与工具

1. 安装node.js
	[下载地址](https://nodejs.org/en/download/)。检验安装成功：

	```
	$ node -v
	```
2. 安装hexo

	```
	$ npm install hexo-cli -g
	```

	NPM是随同NodeJS一起安装的包管理工具，npm 的包安装分为本地安装、全局安装（带-g）两种，这句命令就是全局安装hexo-cli软件。检验安装成功：

	```
	$ hexo -v
	```
3. 安装git和注册github
	略。

# 用hexo搭建一个本地的博客

1. cd命令进入一个目录，执行下面的命令，初始化，会发现多了很多hexo的文件，my_blog_folder_name是文件夹名，随你心意。

	```
	$ hexo init my_blog_folder_name
	```

2. cd命令进入my_blog_folder_name目录

	```
	$ cd my_blog_folder_name
	```

3. 现在你还没有写任何文章，下面这条命令是把source/_posts文件夹的hello-world.md文件生成静态网页。

	```
	$ hexo g
	```

4. 启动本地服务器，用-p指定端口。如果电脑安装了福昕pdf阅读器会占用hexo默认的4000端口，那么指定一个其他的端口。

	```
	$ hexo s -p 7788
	```

5. 浏览器访问 localhost:7788 这时看到的hello world文章。

至此，本地的工作就完成了。你只需要把markdown文档放到_posts路径下，依次执行hexo g、hexo s 就可以本地访问博客。
<!-- more -->
# 把博客推送到github上

github有一个叫github pages的服务，其初衷是给托管其上的项目建立主页。提供300M的免费空间，我们按照规则放置静态网页，便可以访问。下面我们把博客部署到github上。

1. 建立github仓库，仓库名字要求与注册的github帐号一样。

2. 修改hexo配置文件_config.yml 。同名文件有两个，一个是在my_blog_folder_name路径下，是hexo基础配置；另一个是在my_blog_folder_name\themes\landscape路径下，是配置博客主题的。这里用第一个my_blog_folder_name目录下的。

	修改配置文件时注意冒号后有空格。

	```
	deploy: 
		type: git
		repo: git@github.com:your_account_name/your_account_name.github.io.git #这里填入你之前在GitHub上创建仓库的完整路径
		branch: master
	```
	这是给hexo d 这个命令做相应的配置，让hexo知道你要部署到哪，很显然，我们部署在我们GitHub仓库的master分支。

3. 把本地网页文件推到github的仓库上，还需要安装Git部署插件，cd命令进入my_blog_folder_name路径，输入命令：

	```
	npm install hexo-deployer-git --save
	```

4. 最后，执行清理、生成、部署，命令依次是
	
	```
	hexo clean 
	hexo g
	hexo d
	```

	提示 Deploy done: git，表示成功。

5. 浏览器访问，发现你的博客已经上线了，可以在网络上被访问了。


# 使用git管理
现在，你可以在本地写博文、部署、公网访问。但是，文章和hexo的文件都没有用git工具管理起来。明白了原理，加入版本管理轻而易举，只需稍作改变。

我们重新来，重复上边讲过的步骤，但是顺序略有不同。


1. 在github上创建仓库，仓库名与账户名相同。

2. clone仓库到本地。

3. 创建分支myhexo，切换到myhexo分支。分支名随意。

4. cd命令进入本地仓库的目录，hexo init初始化。这里会提示不是空目录，那么创建并进入一个空目录，执行初始化，再把文件copy出来就行。

5. 执行命令hexo g，hexo s -p 7788，本地访问，验证成功。

6. 修改hexo配置文件_config.yml ，与上文一样。

7. 部署，hexo d，公网访问成功。

8. git commit提交修改，git push origin myhexo把本地分支推送到远程。

那么，就可以在myhexo分支管理博文和hexo文件，部署到master分支。


如果需要在另一台电脑上也更新博客，可以这么做：

1. clone仓库到本地。

2. 安装hexo
	```
	npm install hexo
	```

3. themes文件夹下，主题文件没了，需要重新安装