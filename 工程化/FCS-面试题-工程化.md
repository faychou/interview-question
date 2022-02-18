# 面试题-工程化

#### 列举你知道的所有构建工具并说说这些工具的优缺点？这些构建工具在不同的场景下应该如何选型？



#### 简单描述一下 Babel 的编译过程？



#### ESLint 和 Prettier 的区别是什么？两者在一起工作时会产生问题吗？



#### 如果让你设计一个通用的项目脚手架，你会如何设计？一个通用的脚手架一般需要具备哪些能力？



#### 如何使用 Vue CLI 3.x 定制一个脚手架？比如内部自动集成了 i18n、 axios、Element UI、路由守卫等？



#### 前端项目标准？



#### 组件库集成？组件库建设的目的？



#### 代码检查规范 eslint？



#### webpack 插件原理，如何写一个插件？



#### webpack 的 require 是如何查找依赖的？



#### webpack 如何实现动态加载？



#### webpack `treeShaking` 原理，是靠什么才能实现 (ES6 模块的静态导出)。



#### webpack 的构建原理，loader 和 plugin 的区别。



#### git 提交规范？



#### Git 如何修改已经提交的 Commit 信息？



#### Git 如何撤销 Commit 并保存之前的修改？



#### Git 如何 ignore 被 commit 过的文件？



#### 在使用 Git 的时候如何规范 Git 的提交说明（Commit 信息）？



#### Commit 信息如何和 Github Issues 关联？



#### Git Hook 在项目中哪些作用？



#### git fetch 和 git pull 有什么区别，有合并操作吗？



#### git merge 和 git rebase 有什么区别，它们的应用场景有哪些？

- merge会保留两个分支的commit信息，而且是交叉着的，即使是ff模式，两个分支的commit信息会混合在一起（按真实提交时间排序），多用于自己dev合并进master。
- rebase意思是变基，改变分支的起始位置，在dev上`git rebase master`，将dev的多次commit一起拉到要master最新提交的后面(时间最新)，变成一条线，多用于整理自己的dev提交历史，然后把master最新代码合进来。



#### git reset 和 git revert 有什么区别，该如何选择，回滚后的 `<commit-id>` 还能找到吗？

- reset是根据来移动HEAD指针，在该次提交点后面的提交记录会丢失。

- revert会产生新的提交，来抵消选中的该次提交的修改内容，可以理解为“反做”，不会丢失中间的提交记录。