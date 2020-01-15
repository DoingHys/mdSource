[TOC]

# git 初始话关联远程仓库

1. gitHub 创建个人仓库

2. 本地初始化

   ```
   1、git remote add origin [远程url]
   2、git init 
   3、git pull origin master
   4、git push origin master
   ```

   

# git 为本地设置仓库地址

1. 查看远程仓库地址

   `git remote -v` 

2. 关联本地分支地址到远程仓库

   ```
   git remote -v //查看git对应的远程仓库地址
   git remote rm origin //删除关联对应的远程仓库地址
   git remote -v //查看是否删除成功，如果没有任何返回结果，表示OK
   git remote add origin https://github.com/developers-youcong/Metronic_Template.git //重新关联git远程仓库地址
   ```

3. #### 直接修改本地仓库所关联的远程仓库的地址

   ```
   git remote  //查看远程仓库名称：origin 
   git remote get-url origin //查看远程仓库地址
   git remote set-url origin https://github.com/developers-youcong/Metronic_Template.git  ( 如果未设置ssh-key，此处仓库地址为 http://... 开头)
   ```

   

# 提取分支合并(cherry-pick)

需求场景：

假设你现在正在开发一个项目，有一个功能分支 feature，开发分支 develop。 feature 有3个提交，分别是 A ，B ，C 。develop 分支只想加入 C 功能， 此时合并操作无法满足，因为直接合并 feature，会将3个提交都合并上，我想合并就只有 C，不要 A，B。此时就需要挑樱桃大法–cherry pick！

**操作步骤：**

1. 切换到develop 分支
2. 通过 git log feature 找到C 的 SHA1 值（commit 的值）
3. 通过 git cherry-pick SHA1 ，将C 的代码合并到当前develop 中

**更多操作：**

1. 挑选多个合并

```git cherry-pick A B
git cherry-pick A B
例如：git cherry-pick 4d2951 e4cdff9
```

2. 连续合并(左开右闭原则)

```
git cherry-pick ^commit1 commit10
```

3、当出现冲突显示 **cherry-pick|merging** 时可以 使用以下命令

```
1）想回退到cherry 前的版本时:git cherry-pick --abort
2) 想继续时：git cherry-pick --continue
3)中断：git cherry-pick --quit
```



# 出现代码冲突时，用TortoiseGIt 解决

参考博客：**https://www.cnblogs.com/jasongrass/p/11199039.html**



# 分支代码回滚

**参考博客：https://www.jianshu.com/p/9ae06b0ddee5 **

```
1、git reflog
2、找到自己需要回退得head 执行
3、git reset --hard@{0}
```

