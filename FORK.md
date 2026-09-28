# Gs-ygc fork of dsh-context

基线: upstream tag `v0.55.0` + 本仓库 `stable` 分支上的 overlay（直接提交构建产物 lib/，安装即用、无需 build）。

## Overlay 内容（相对上游 v0.55.0）
- 工具调用账本 `toolOps`（每次调用：耗时/输出/token/状态/raw 命令/进程归因 procs）
- 进程树资源采样（CPU/RSS，per-PID delta + 生命周期归因，采样器自身 ps 排除）
- 标签系统：dshctx_tags 存储域 + /api/dsh-context/tags 路由 + annotate_tool_call 工具
- 定时增量 AI 归因（空队列不跑 LLM，写 ai: 前缀标签，attribIntervalMs 可配）
- 语义工具名（codex_command→bash 等映射）+ procs 归因名显示
- toolOpSchema 扩展 raw/procs/cpuMs/rssGrow/approx；stateVersion=22

## 跟上游合并新东西
1. `git fetch upstream --tags`
2. `git checkout -b stable-<ver> v<ver>`，把本分支 overlay 搬过去（初版可 `git checkout stable -- lib package.json FORK.md ...` 再按上游新结构手工对齐 src 语义）
3. 验证：dev profile 换分支名 → 面板各卡片/ai 标签/资源行正常
4. 合并后更新本文件

## 已知偏差
- 上游 0.59.0 锚点已漂移（pushToolOp/installTags 等不存在），不能直接 git apply 旧 patch；本 fork 以 lib 产物形式维护，升级=在新 tag 上重放 overlay。
