# xwiki 在线实验环境

## 软件简介

软件基本信息，License，所属的类别，作用，特点等。

XWiki是一个以Java编写的免费wiki软件平台，其设计重点在于可扩展性。XWiki是企业的维基。它包括WYSIWYG编辑，基于OpenDocument的文档导入/导出，语义注释和标记以及高级权限管理

所属类别是建站系统

特点：

* 内容管理(浏览/编辑/预览/保存)

* 支持附件

* 版本控制

* 全文本搜索

* 强大的权限管理

* 使用Hibernate进行数据存储

* RSS输出与显示外部的RSS feeds

* 多语言支持

* 提供XML/RPC的API

* WYSIWYG HTML编辑器

* 导出为PDF

* Groovy脚本支持等

## 软件官网

http://www.xwiki.org/xwiki/bin/view/Main/WebHome

## Dockerfile 使用方法

### 如何使用
您应该首先在您的机器上安装Docker。

那么有几个选择：

- 从DockerHub拉出xwiki图片
 获取此项目的来源并构建它们
### 拉现有图像
你需要运行2个容器：

- 一个为XWiki图像
- 一个用于XWiki连接到的数据库映像
### 使用码头运行
首先创建专门的码头网络：
```
docker network create -d bridge xwiki-nw
```
然后为数据库运行一个容器，并确保配置为使用UTF8编码。以下数据库开箱即用：

- MySQL
-   PostgreSQL
启动MySQL
以下命令还将配置MySQL容器，以将其数据保存在localhost上的/my/own/mysql目录中：
```
docker run --net=xwiki-nw --name mysql-xwiki -v /my/own/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=xwiki -e MYSQL_USER=xwiki -e MYSQL_PASSWORD=xwiki -e MYSQL_DATABASE=xwiki -d mysql:5.7 --character-set-server=utf8 --collation-server=utf8_bin --explicit-defaults-for-timestamp=1
```
您应该调整命令行以使用您希望获得MySQL根密码和xwiki用户密码的密码。

启动PostgreSQL

下面的命令还将配置PostgreSQL容器，以便将其数据保存在localhost上的/my/own/postgres目录中：
```
docker run --net=xwiki-nw --name postgres-xwiki -v /my/own/postgres:/var/lib/postgresql/data -e POSTGRES_ROOT_PASSWORD=xwiki -e POSTGRES_USER=xwiki -e POSTGRES_PASSWORD=xwiki -e POSTGRES_DB=xwiki -e POSTGRES_INITDB_ARGS="--encoding=UTF8" -d postgres:9.5
```
您应该调整命令行以使用您希望PostgreSQL根密码和xwiki用户密码的密码。

### 开始XWiki
然后通过发出以下命令之一运行XWiki在另一容器中。

对于MySQL：
```
docker run --net=xwiki-nw --name xwiki -p 8080:8080 -v /my/own/xwiki:/usr/local/xwiki -e DB_USER=xwiki -e DB_PASSWORD=xwiki -e DB_DATABASE=xwiki -e DB_HOST=mysql-xwiki xwiki:mysql-tomcat
```
对于PostgreSQL：
```
docker run --net=xwiki-nw --name xwiki -p 8080:8080 -v /my/own/xwiki:/usr/local/xwiki -e DB_USER=xwiki -e DB_PASSWORD=xwiki -e DB_DATABASE=xwiki -e DB_HOST=postgres-xwiki xwiki:postgres-tomcat
```
请务必使用与第一个命令中使用的相同的DB用户名，密码和数据库名称来启动DB容器。另外，请不要忘记-e DB_HOST=使用以前创建的DB容器的名称添加环境变量，以便XWiki知道数据库的位置。

此时，XWiki应以交互式阻止模式启动，允许您在控制台中查看日志。如果您希望在“分离模式”下运行，只需在上一个命令中添加一个“-d”标志即可。
```
docker run -d --net=xwiki-nw ...
```
## 资源链接

- http://www.xwikichina.com/xwiki/bin/view/Main/
- http://www.xwikichina.com/xwiki/bin/view/Features/
