[TOC]

# 提取分支合并(cherry-pick)

需求场景：

假设你现在正在开发一个项目，有一个功能分支 feature，开发分支 develop。 feature 有3个提交，分别是 A ，B ，C 。develop 分支只想加入 C 功能， 此时合并操作无法满足，因为直接合并 feature，会将3个提交都合并上，我想合并就只有 C，不要 A，B。此时就需要挑樱桃大法–cherry pick！

**操作步骤：**

1. 切换到develop 分支
2. 通过 git log feature 找到C 的 SHA1 值（commit 的值）
3. 通过 git cherry-pick SHA1 ，将C 的代码合并到当前develop 中

**更多操作：**

1. 挑选多个合并





# 分支代码回滚

**参考博客：https://www.jianshu.com/p/9ae06b0ddee5 **

```
1、git reflog
2、找到自己需要回退得head 执行
3、git reset --hard@{0}
```

