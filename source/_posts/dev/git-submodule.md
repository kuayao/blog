---
layout: dev
title: Git Submodule 子项目全流程使用指南
date: 2024-08-07 22:41:28
tags: git
---
Git Submodule 是 Git 中用于管理子项目的强大功能。它允许我们将一个 Git 仓库作为另一个 Git 仓库的子模块进行管理，从而使项目结构更加清晰，代码维护更加方便。   

本指南将详细讲解 Git Submodule 的创建、规划、更新、合并全流程的使用过程和操作步骤，可以更好地理解和使用 Submodule。

1. 创建 Submodule
    1. 初始化主项目 
        我们需要初始化一个主项目仓库
        ```shell 
            git init <主项目名称>
        ```
    2. 添加子模块
        使用 **git submodule add** 命令添加子模块
        ```shell
            git submodule add <子模块 URL> <子模块目录>
        ```
        例: 将名为 **lib** 的子模块添加到 **main** 项目中
        ```shell
            git submodule add git@github.com:user/lib.git lib
        ```
    3. 提交变更
        提交添加子模块的变更
        ```shell
            git commit -m '添加子模块 lib'
        ```
2. 规划 Submodule
    1. 子模块版本控制
        我们可以像管理主项目一样管理子模块版本
        - 克隆子模块
            * **--init** 选项: 如果子模块尚未初始化，则将其初始化
            * **--recursive**  选项: 递归更新所有子模块，包括嵌套子模块
                该命令将执行以下操作:
                1. 初始化所有未初始化的子模块
                2. 更新所有子模块到最新提交
                3. 递归更新所有嵌套子模块
            ```shell
                git submodule update --init --recursive
            ```
        - 更新子模块
            ```shell
                git submodule update --recursive
            ```
        - 提交子模块变更  
            ```shell
                cd lib
                git add .
                git commit -m "更新子模块 lib"
                cd ..
                git submodule add lib
                git commit -m "更新子模块版本"
            ```
    2. 子模块分支管理
        子模块可以独立进行分支管理
       - 切换子模块分支
            ```shell
                git submodule checkout <分支名称>
            ```
        - 创建子模块分支
            ```shell
                git submodule branch <分支名称>
            ```
        - 合并子模块分支
            ```shell
                git submodule merge <分支名称>
            ```
3. 更新 Submodule
    1. 更新所有子模块
        我们可以使用 **git submodule update** 命令更新所有子模块
        ```shell
            git submodule update --recursive
        ```
    2. 更新指定子模块
        使用 **git submodule update** 命令更新指定的子模块
        ```shell
            git submodule update <子模块目录>
        ```
        例：更新 **lib** 子模块
        ```shell
            git submodule update lib
        ```
    3. 单独更新子模块
        使用 **git fetch** 和 **git reset** 命令单独更新子模块
        ```shell
            git fetch <子模块 URL>
            git reset --hard <子模块版本>
        ```
        例：将 **lib** 子模块更新到 **v1.0.0** 版本
        ```shell
            git fetch git@github.com:user/lib.git
            git reset --hard v1.0.0
        ```
4. 合并 Submodule
    1. 合并子模块变更
        当子模块发生变更时，需要将其合并到主项目中
        ```shell
            git submodule update --init --recursive
            git add .
            git commit -m "合并子模块变更"
        ```
    2. 解决冲突
        如果子模块更新导致冲突，需要手动解决冲突
        ```shell
            git submodule status
            git submodule foreach git mergetool
            git add .
            git commit -m "解决子模块冲突"
        ```
5. 高级用法
    1. 子模块指针
        使用子模块指针来指定子模块的特定版本
        ```shell
            git submodule add --depth 1 git@github.com:user/lib.git lib
            git submodule update --init --recursive
        ```
    2. 子模块克隆
        使用 **git submodule clone** 命令克隆子模块到单独的目录。
        ```shell
            git submodule clone git@github.com:user/lib.git lib
        ```
    3. 子模块删除
        使用 **git submodule deinit** 和 **git rm** 命令删除子模块
        ```shell
            git submodule deinit lib
            git rm -rf lib
        ```
**注意：** 使用 Submodule 时需要注意以下几点
 * 子模块的更新可能会导致项目冲突，需要及时解决
 * 子模块的版本管理需要纳入项目的整体规划