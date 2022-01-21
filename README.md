# 实操 Git

> 在这个库里通过实际操作学会正确的 git 规范化流程

## 目录

- [前言](https://github.com/masterX89/learn-git#%E5%89%8D%E8%A8%80)
  - [git 优点](https://github.com/masterX89/learn-git#git-%E4%BC%98%E7%82%B9)
  - [惧怕 git 的原因](https://github.com/masterX89/learn-git#%E6%83%A7%E6%80%95-git-%E7%9A%84%E5%8E%9F%E5%9B%A0)
  - [黄金法则](https://github.com/masterX89/learn-git#%E9%BB%84%E9%87%91%E6%B3%95%E5%88%99)
- [前置环境](https://github.com/masterX89/learn-git#%E5%89%8D%E7%BD%AE%E7%8E%AF%E5%A2%83)
- [流程](https://github.com/masterX89/learn-git#%E6%B5%81%E7%A8%8B)
  - [普通流程](https://github.com/masterX89/learn-git#%E6%99%AE%E9%80%9A%E6%B5%81%E7%A8%8B)
  - [修改 commit](https://github.com/masterX89/learn-git#%E4%BF%AE%E6%94%B9-commit)
    - [修改 commit message](https://github.com/masterX89/learn-git#%E4%BF%AE%E6%94%B9-commit-message)
    - [合并 commits](https://github.com/masterX89/learn-git#%E5%90%88%E5%B9%B6-commits)
- [学习资料](https://github.com/masterX89/learn-git#%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99)

## 前言

### git 优点

1. 分布式。不用像 svn 那样需要时刻保持联网或者连接服务器状态开发。
2. 分支管理。非集成分支的功能和修复bug不会即刻影响到线上代码，只有通过了检查、测试和代码评审的代码才有资格合并入集成分支。
3. 本地检查。提交代码前，husky 对代码进行静态检查和测试用例是否通过。
4. 提交检查。commit lint 可以约束 message 的规范，从而确保日志的正确生成。
5. 版本日志。通过 commit message 可以自动生成版本日志。
6. ci/cd。action 会对代码进行静态检查和测试。
7. code review。
8. Bug 排查。在排查错误的时候，使用 git 可以二分快速定位。

​	......

### 惧怕 git 的原因

1. 对 git 的不了解。
   - 不了解 git 原理
   - 不了解每个指令的用途
   - 不了解指令内部参数的用法
2. 对交互式界面的不熟悉
   - 不会操作 vim

### 黄金法则

#### 严禁在集成分支使用 `git push -f`

`force-push` 该操作意味着无视**远程分支**，强行使**远程分支**的内容和**本地分支**同步，效果就是***删库跑路***。所以请不要在**集成分支**上使用该命令。

#### 严禁在集成分支使用 `git rebase`

变基操作会修改**基**后面的所有 commit 信息，使用该操作会使得团队内所有成员的本地分支内容和远程分支产生差异。所以请不要在**集成分支**上使用变基操作。

## 前置环境

### 安装 git

```bash
$ sudo apt-get install git
# 检查 git 是否安装成功
$ git --version
> git version 2.25.1
```

### git 最小配置

```bash
# 配置全局用户名 xxx 替换为你的用户名
$ git config --global user.name xxx
# 配置全局email xxx@xxx.com 替换为你的邮箱
$ git config --global user.email xxx@xxx.com
# 检查是否配置成功
$ git config --list --global
> user.name=xxx
> user.email=xxx@xxx.com
```

### 配置 github ssh 密钥

> 注意：一般来说，开发环境需要同时管理 github 开源库和 gitlab 内部库两个 ssh 密钥，所以可以在 [github 官网](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)的基础上使用 config 进行管理

```bash
$ cd ~/.ssh
# 生成密钥
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# 在取名步骤需要取名为 github 相关
> Enter file in which to save the key: id_rsa_github
# 检查密钥
$ ls
> id_rsa_github  id_rsa_github.pub  known_hosts
# 修改配置文件
$ vim config
# 添加以下内容
Host github.com
     HostName github.com
     User git
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/id_rsa_github
# 复制你的公钥内容
$ cat id_rsa_github.pub
```

打开你的[github设置界面](https://github.com/settings/profile)在 ssh 配置中新建一个 ssh key 并将公钥内容粘贴进去

```bash
# 验证 ssh 配置
$ ssh -T git@github.com
> Hi your_name! You've successfully authenticated, but GitHub does not provide shell access.
```

## 流程

### 普通流程

1. Fork 这个库

2. 将这个库 clone 到本地

   ```bash
   $ git clone git@github.com:masterX89/learn-git.git
   ```

3. 新建一个分支

   ```bash
   $ git switch -c feat/add-new-feature
   ```

4. 向模拟代码文件中添加新功能

   ```bash
   $ vim feat.md
   # 添加以下内容
   - this is a new feat
   ```

5. 将代码提交至暂存区

   ```bash
   # 或者使用 git add feat.md
   $ git add .
   ```

6. 本地提交 commit

   ```bash
   $ git commit -m "feat: add a new feature"
   ```

7. 同步远程代码并处理冲突

   ```bash
   $ git pull --rebase origin main
   # 和同事协商处理冲突
   ```

8. 推送至远程

   ```bash
   $ git push -f
   ```

9. 发起 PR

### 修改 commit

#### 修改 commit message

#### 合并 commits

## 学习资料

- [MIT 课程：计算机教育中缺失的一课](https://missing.csail.mit.edu/)
- [上海交大 ipads 新人培训](https://www.bilibili.com/video/BV1YR4y1E7LX?share_source=copy_web)
- [Learning Git Branching 在线沙盒](https://learngitbranching.js.org/)