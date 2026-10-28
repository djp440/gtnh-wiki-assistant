# 任务跟踪：GitHub

任务与规格保存在 `djp440/gtnh-wiki-assistant` 的 GitHub Issues，
所有操作使用 `gh`。

## 操作约定

在仓库目录执行命令。每次会话首次写入前，使用
`gh repo view --json nameWithOwner` 确认目标为本仓库。

- 创建：`gh issue create --title "..." --body-file <正文文件>`
- 读取：`gh issue view <编号> --json number,title,body,labels,comments`
- 列表：`gh issue list --state open --json number,title,body,labels`
- 更新正文：`gh issue edit <编号> --body-file <正文文件>`
- 评论：`gh issue comment <编号> --body-file <正文文件>`
- 添加标签：`gh issue edit <编号> --add-label "<标签>"`
- 移除标签：`gh issue edit <编号> --remove-label "<标签>"`
- 关闭：先发表评论说明原因，再执行 `gh issue close <编号>`。

多行正文写入 UTF-8 文件，通过 `--body-file` 传递。

## 子任务与依赖

- 创建子任务：`gh issue create --parent <父编号> ...`
- 关联已有子任务：`gh issue edit <父编号> --add-sub-issue <子编号>`
- 添加阻塞关系：`gh issue edit <子编号> --add-blocked-by <阻塞编号>`
- 所有阻塞任务关闭后，任务才可开始。

原生关系不可用时，在子任务正文顶部记录 `Part of #<父编号>`
和 `Blocked by: #<编号>`，并在父任务中维护子任务清单。

## 发布与读取

技能要求“发布到任务跟踪系统”时，创建 GitHub Issue。
要求“读取相关工单”时，按编号读取 Issue 及其评论。

## Pull requests as a triage surface

**PRs as a request surface: no.**

## Wayfinding

- 地图：使用带 `wayfinder:map` 标签的 Issue，
  正文维护 Notes、Decisions-so-far 和 Fog。
- 子任务：关联到地图，使用 `wayfinder:<type>` 标签；
  type 为 research、prototype、grilling 或 task。
- 可领取任务：按地图顺序选择未关闭、无未关闭阻塞、无人负责的子任务。
- 领取：先执行 `gh issue edit <编号> --add-assignee @me`。
- 完成：评论答案并关闭子任务，再把结论摘要与链接追加到地图的
  Decisions-so-far。
