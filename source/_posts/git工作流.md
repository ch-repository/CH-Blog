---
title: git工作流
date: 2023-05-23 15:03:59
categories:
  - 开发工具
tags:
  - Git
comments: false
---

##  Git常见工作流
Git三中常见的工作流：Git Flow、GitHub Flow、GitLab Flow

## Git Flow
Git Flow是最早诞生也是最早被广泛使用的工作流程。

在Git Flow中，有两个长期存在且不会被删除的分支：master和develop。

在这两个分支中，master主要用于对外发布稳定的新版本，该分支时常保持着软件可以正常运行的状态，由于要维护这一状态，所以不允许开发者直接对master分支的代码进行修改和提交。其他分支的开发工作进展到可以发布的程度后，将会与master分支进行合并，并且这一合并只在发版时进行，发布时会附加版本编号的Git标签。

develop则用来存放我们最新开发的代码，这个分支是我们开发过程中代码的中心分支，这个分支也不允许开发者直接进行修改和提交。程序员要以develop分支为起点新建feature分支，在feature分支中进行新功能的开发或者代码的修正，也就是说develop分支维系着开发过程中的最新代码，以便程序员创建feature分支进行自己的工作。

> 注意：develop合并的时候，不要使用fast-farward merge，建议加上 --no-ff 参数，这样在master上就会有合并记录。

除了这两个永久分支，还有三个临时分支：feature、hotfixes和release

### feature
这个是特性分支，也叫功能分支，当你需要开发一个新的功能的时候，可以新建一个feature-xxx的分支，在里边开发新功能，这也是我们日常工作的大本营，开发完成后，合并到develop分支中，如下图：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231513492.png" />

### hotfixes
这个分支开名字就是用来修复bug的，当我们的项目上线后，发现有bug需要修复，那么就从master上拉一个名为fixbug-xxx的分支，然后进行bug修复，修复完成后，再将代码合并到master和develop两个分支中，然后删除hotfix分支，如下图：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231516950.png" />

### release

这个是发版的时候拉的分支，当我们所有的功能做完之后，准备将代码合并到master的时候，从develop上拉一个release-xxx分支出来，这个分支一般处理发版前的一些提交以及客户体验之后小bug的修复（bug修复也可以合并进develop），不要在这个里面去开发功能，在预发布结束后，将该分支合并进develop以及master，然后删除release，如下图：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231518277.png" />

## GItHub Flow

GitHub Flow相比与Git Flow就要容易多了，GitHub Flow也是GitHub上使用的工作流程，如果你想参与GitHub上的某一个开源项目，那么不妨看看GitHub Flow。官方给的GitHub Flow流程如下：

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231521597.png" />

- 第一步：根据需求，从master拉出新分支，不区分功能分支或补丁分支。
- 第二步：新分支开发完成后，或者需要讨论的时候，就向master发起一个pull request。
- 第三步：pull request即是一个通知，让别人注意到你的请求，又是一种对话机制，大家一起评审和讨论你的代码。对话过程中，你还可以不断提交代码。
- 第四步：你的pull request被接受，合并进master，重新部署后，原来你拉出来的那个分支就被删除。（先部署再合并也可）

GItHub工作流虽然用这很简单，但是它的问题也很明显，就是没有对常见的工作场景中的问题提出解决办法。GitHub Flow这种方式，要保证高质量，对于贡献者的素质要求很高，换句话说，就是代码贡献者素质不那么高，质量就无法得到保证。



## GitLab Flow

GitLab Flow是Git Flow与GitHub Flow的结合。它吸取了两者的优点，既有适应不同开发环境的弹性，又有单一主分支的简单和便利。他是GitLab.com推荐的做法。



GitLab Flow的最大原则叫做“上游优先”，即只存在一个主分支master，它是所有其他分支的“上游”。只有上游分支采纳的代码变化，才能应用到其他分支。



对于“持续发布”的项目，它建议在master分支以外，再建立不同的环境分支。比如，“开发环境”的分支是master，“预发环境”的分支是pre-production，“生产环境”的分支是production。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231532343.png" />

只有紧急情况，才允许跳过上游，直接合并到下游分支。对于“版本发布”的项目，建议的做法是每一个稳定版本，都要从master分支拉出一个分支，比如2-3-stable、2-4-stable等等。

<img src="https://blog-image-ch.oss-cn-hangzhou.aliyuncs.com/blog-image/202305231534081.png" />

GitLab Flow如何处理hotfix？Git Flow之所以这么复杂，一大半原因就是把hotfix考虑的太周全了。hotfix的意思是，当代码部署到环境之后发现的问题，需要火速fix。GitLab Flow可以基于后续分支，修改后上线。

#### 采用GitLab Flow工作流程

- 新的迭代开始，所有开发人员从主干master拉个人分支开发特性，分支命名规范feature-name
- 开发完成后，在迭代结束前，合入master分支
- master分支合并后，自动cicd到dev环境
- 开发自动通过后，从master拉取药发布的分支，release-$version，将这个分支部署到测试环境测试
- 测试的bug，通过从release-$version拉出分支进行修复，修复完成后，再合入release-$version
- 正式发布版本，如果上线后，又有bug，根据5的方式处理
- 等发布版本稳定后，将release-$versin反合入主干



转自：https://www.jianshu.com/p/7eba1f0b5b42