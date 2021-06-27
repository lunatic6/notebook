# 1. 介绍

# 2. Git命令行操作

## 1.本地库初始化

1. 命令：`git init`
2. 注：`.git`目录中存放的是本地库相关的子目录和文件，不要删除，也不要修改。

## 2. 设置签名

- 形式：
  - user：juxinbo
  - email：juxinbo@bytedance.com
- 作用：区分不同开发人员的身份
- 说明：此处的签名，和登陆远程库（github）的账号、密码没有任何关系
- 命令：
  - 项目/仓库级别：仅在当前本地库范围内有效
    - git config user.name juxinbo
    - git config user.email juxinbo@bytedance.com
  - 系统用户级别：登陆当前操作系统的用户范围
    - git config **--global** user.name juxinbo
    - git config **--global** user.email juxinbo@bytedance.com
    - 信息保存位置：~/.gitconfig，用户家目录下的
  - 优先级：项目级别有限，二者都没的话报错
- 注：可以直接通过~/.gitconfig设置

## 3. 添加，提交，查看状态

1. 添加：

   - 命令：git **add**
   - 将工作区的**新建/修改**添加到缓存区
   - 缓存区有绿色的文件，表示还没commit到本地库

2. 提交

   - 命令：git **commit** -m file
     - 注：不加参数-m，会进入vim编辑一段提交信息；使用git commit -m “要说明的信息” file，这样不用进入vim中
   - 将暂存区的内容提交本地库

3. 查看状态

   - 命令：git **status**

   - 查看工作区，暂存区的状态

       									  <img src="D:\Desktop\Markdown\Git\提交流程说明.jpeg" alt="提交流程说明" style="zoom: 67%;" />

   - 注：第一次创建的文件，必须先add，后续修改的文件，可以直接commit；但是因为没有提交到暂存区，所以无法撤销
     - 咋撤销？？

4. 查看提交记录

   - 命令：git **log**

     ​			 ![log1](D:\Desktop\Markdown\Git\log1.png)

