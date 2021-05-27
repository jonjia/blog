**本文不是对 Git 的完整介绍，只是想说明学习理解 Git 相关概念的重要性。理解了 Git 的术语和模式后，对 Git 的使用会事半功倍**

## 概念简介

#### 术语

- 分布式（distributed）：代码仓库**分布**在不同的客户端和服务端，每个地方都包括完整的数据、完整的历史记录
- 校验和（checksumming）：数据的引用（可以理解为数据的 key）。由 40 个十六进制字符（0-9 和 a-f）组成的字符串，基于 Git 中文件的内容或目录结构计算出来，比如：`24b9da6552252987aa493b52f8696cd6d3b00373`
- 快照（snapshot）：一个 commit 就是一份快照，包括从项目根目录开始的所有文件和目录
  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aea543dad5724c16b57e4ae7ae19067e~tplv-k3u1fbpfcp-zoom-1.image)
  > _图片来自 https://blog.sogilis.com/posts/2015-05-05-demystifying-git-concepts-to-understand/_
- 变更集（changeset）：新 commit 引入的所有变化
  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c92a621026244ec8dd0221f6c7b0489~tplv-k3u1fbpfcp-zoom-1.image)
  > _图片来自 https://blog.sogilis.com/posts/2015-05-05-demystifying-git-concepts-to-understand/_
- 分离头指针（detached HEAD）：脱离了分支或者 tag 的 HEAD，这种情况下创建的 commit 可能会丢失

#### 文件的三种状态

1. 已修改（modified）表示修改了文件，但还没保存到数据库中
2. 已暂存（staged）表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中
3. 已提交（committed）表示数据已经安全地保存在本地数据库中

#### 三个区域（与三种状态一一对应）

1. 工作区是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改
2. 暂存区保存了下次将要提交的信息，一般在 Git 仓库目录中
3. Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方
   ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d9b021a3f3b246d2ab34f4f1d88b2c7b~tplv-k3u1fbpfcp-zoom-1.image)
   > _图片来自 https://git-scm.com/_

## 命令简介

#### 文件操作

- `git add` 跟踪新文件、暂存已修改的文件
- `git commit` 提交暂存区更新
- `git rm <filename>` 移除文件
- `git rm --cached <filename>` 移除 Git 对文件的跟踪，也可以用于取消暂存
- `git reset HEAD <filename>` 将文件修改为指定状态
- `git mv <filename> <new-filename>` 文件重命名
- `git checkout` 切换分支或重置工作区内容
- `git clean` 移除未跟踪文件
- `git restore` 恢复文件
- `git help <command>` 帮助信息

#### 比较：

- `git diff` 比较工作目录和暂存区之间的差异
- `git diff --cached` 或 `git diff --staged` 比较暂存区与 HEAD 之间的差异
- `git diff HEAD` 比较工作区与 HEAD 之间的差异
- `git diff branch1 branch2` 比较不同 commit 之间的差异

## .git

- HEAD 文件：HEAD 文件通常是一个符号引用（symbolic reference），指向目前所在的分支。 所谓符号引用，表示它是一个指向其他引用的指针
- refs 目录: tags、heads、origin
- objects 存放对象数据，对象类型: 数据对象（blob object）、树对象（tree object）、提交对象（commit object）、标签对象（tag object）
- blob 对象和文件名没有关系，只取决于文件内容，如果两个文件内容相同，就是同一个 blob。可以节约存储空间
- 对同一文件多次修改提交后,会产生多个 Blob 对象？Git 也考虑到这个问题了，所以它会把松散的 blob 做整理，把内容相近的 blob 做增量存储

## Git 工作流

#### 业界 Git 工作流

- Git Flow
- GitHub Flow
- GitLab Flow
- Trunk-based Flow
- Aone Flow

#### Git 工作流选择依据

1. 团队规模、团队人员
2. 产品特点、产品形态、产品规模、产品发布方式
3. 合作团队诉求
4. 基础设施状况

## 推荐资料

1. [Git-scm](https://git-scm.com)
2. [Git concepts simplified](<https://gitolite.com/gcs.html#(8)>)
3. [3 Concepts to Understand the Git Model](https://blog.sogilis.com/posts/2015-05-05-demystifying-git-concepts-to-understand/)
4. [3 Concepts to Do Everything with Git](https://blog.sogilis.com/posts/2015-05-12-demystifying-git-concepts/)
5. [Git for Computer Scientists](https://eagain.net/articles/git-for-computer-scientists/)
6. [What are GitLab Flow best practices?](https://about.gitlab.com/topics/version-control/what-are-gitlab-flow-best-practices/)
7. [字节研发设施下的 Git 工作流](https://juejin.cn/post/6875874533228838925#heading-20)
