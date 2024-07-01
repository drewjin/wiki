---
title: "Git 速通指南 | Part 3: 作为协作者贡献"
---

> 📌**注意**: 对于 SHUSCT, 我们倾向使用 GitHub Flow 进行协作. 详细内容请参考 [GitHub Flow](https://guides.github.com/introduction/flow/).

## 1. 指定协作者 (Collaborator)

1. 打开你的项目仓库, 点击 `Settings` 选项卡.
2. 在左侧导航栏中点击 `Manage access`.
3. 在右上角点击 `Invite a collaborator`.
4. 输入协作者的 GitHub 用户名, 点击 `Add [username] to [repository]`.

## 2. `clone` 项目仓库

参考 [Git 速通指南 | Part 1: 基础 - 4. 远程储存库](../git-speedrun-guide-part-1-basics/#4-远程-git-储存库) 中的内容, 将项目仓库 `clone` 至本地.

## 3. 查看 `issue`

在项目仓库中点击 `Issues` 选项卡. 

选择一个你感兴趣的 `issue`, 并在评论中表明你想要解决这个 `issue`. 你可以提出你的解决方案, 或者询问其他协作者的意见; 你还可以将 `issue` 分配给自己.

如果没有与你想法相关的 `issue`, 你可以创建一个新的 `issue`.

## 4. 创建分支

当你决定解决一个 `issue` 时, 请先检查是否已经有合适的分支. 

如果有, 可以直接在本地 `pull` 并 `switch` 到该分支进行工作:

```bash
# Pull the new version from the remote repository
git pull origin <BranchName> --no-rebase
# Switch to the branch
git switch <BranchName>
```


如果没有合适的分支, 你需要创建一个新的分支. 我们建议使用如下命名规范:

<p align="center">

| 分支名 | 用途 |
| :---: | :---: |
| `feature/#<IssueNumber>-<Description>` | 添加新功能 |
| `bugfix/#<IssueNumber>-<Description>` | 修复 bug |
| `hotfix/#<IssueNumber>-<Description>` | 紧急修复 |
| `doc/#<IssueNumber>-<Description>` | 文档更新 |

</p>

参考 [Git 速通指南 | Part 2: 分支于子模块 -  1. 分支](../git-speedrun-guide-part-2-branch-and-submodule/#1-分支) 创建新分支并推送至远程仓库. 例如, 如果你想解决 `issue #1` (关于支持 `std::format`), 你可以创建一个名为 `feature/#1-support-std-format` 的分支并推送至远程仓库; 参考以下命令:

```bash
# Switch to the target branch
git switch main  # Take main branch as an example
# Create and switch to a new branch; The branch is based on the target branch
git switch -c feature/#1-support-std-format
# Push the new branch to remote repository
git push origin feature/#1-support-std-format:feature/#1-support-std-format
```

## 5. `push` 更改

修改完成后, 请将更改 `add` 和 `commit` 到本地仓库, 接着先 `pull` 远程仓库的最新更改 (他人可能在你提交之前 `push` 到了你正在工作的分支, 你需要 `pull` 下来进行可能的[冲突处理](../git-speedrun-guide-part-2-branch-and-submodule/#13-冲突处理)), 然后再 `push` 你的更改至远程仓库:

```bash
# Add local changes to the staged area
git add .
# Commit local changes to the local Git repository; Now there is a new version of your project
git commit -m "<Message>"
# Pull the new version from the remote repository
git pull origin <BranchName> --no-rebase
# [Note]: If there are conflicts, you need to resolve them manually.
# Push the new version to the remote repository
git push origin <BranchName>
```

## 6. 提交 `PR`

当你的更改完成并 `push` 到远程仓库后, 请在 GitHub 上提交一个 `PR` (Pull Request).

1. 打开项目仓库, 点击 `Pull requests` 选项卡.
2. 点击 `New pull request`.
3. 选择你的分支和目标分支. 由于我们采用 GitHub Flow, 通常目标分支是 `main`; `PR` 操作的目的是将你的分支 `merge` 入目标分支.
4. 输入 `PR` 的标题和描述. **如果你的 `PR` 关联到某个 `issue`, 请在描述中提及, 例如: `Fix #1`, `Close #1`**.
5. 点击 `Create pull request`.

如果 GitHub 提示 `conflict`, 那么表示当前分支和目标分支间有无法自动解决的冲突. 以目标分支为 `main` 为例, 你需要将最新的 `main` 分支 `merge` 入你的分支, 并解决冲突后再次 `push` 到远程仓库; 参考以下代码:

```bash
# Switch to the target branch
git switch main
# Pull the new version from the remote repository
git pull origin main --no-rebase
# Switch to your branch
git switch <BranchName>
# Merge the target branch into your branch
git merge main
# [Note]: If there are conflicts, you need to resolve them manually.
# Push the new version to the remote repository
git push origin <BranchName>
```

此时你的分支能够被 `merge` 入目标分支; 回到刚才的 `PR` 页面, GitHub 能够自动检测到你的分支已经能够被 `merge`.

## 7. `PR` 审核与合并

作为协作者, 你可以在 `PR` 页面查看其他协作者提交的 `PR`, 并对其进行评论. 你可以提出修改建议, 或者直接 `approve` 该 `PR`.

当 `PR` 经过多人 `approve` 后, 项目维护者可以 `merge` 该 `PR`. 项目维护者可以选择 `Squash and merge`, `Rebase and merge`, 或者 `Create a merge commit` 等方式进行 `merge`.

## 8. `Tag`

如果想要将某个版本标记为重要版本, 可以使用 `tag`. 例如, 如果你的项目完成了一个重要的版本迭代, 你可以使用 `tag` 标记该版本:

```bash
# Switch to the target branch
git switch main  # Take main branch as an example
# Tag the version
git tag -a v1.0 -m "Release v1.0"
# Push the tag to the remote repository
git push origin v1.0
```


