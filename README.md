# Converge Code Changes

一个帮助 Coding Agent **停止反复打补丁，并把代码收敛到最终状态**的 Codex Skill。

它解决的不是普通“代码清理”，而是代码修改过程中常见的累积问题：

```text
Bug
→ 加入方案 A
→ 用户否决 A
→ 再叠加方案 B
→ 留下“不使用 A”的注释
→ 继续增加 fallback、兼容分支和临时判断
```

`converge-code-changes` 要求 Agent 重新计算用户的**最新净需求**，修正原实现，并让代码、测试、样式和文档共同收敛为一个当前状态。

## 它会改变什么

- 修复违反约束的源头，而不是只在下游增加空值、默认值或异常兜底。
- 用户改变方案时替换原实现，不叠加第二套分支、wrapper 或覆盖样式。
- 把被否决的方案视为对话历史，不在代码中留下“不要使用旧方案”之类的注释。
- 清理补丁时保留已经确认的正确行为，只移除 workaround、旧分支和过程叙述。
- 删除功能或 UI 内容时，同时清理相关 import、状态、样式、边框、分隔线、光晕、留白、测试和文档。
- 只有现实中的当前契约确实需要时，才保留 fallback 或 compatibility layer。
- 完成修改后检查完整 diff，确保只剩一条 canonical implementation path。

## 安装

把仓库克隆到 Codex 的个人技能目录：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/Mori-17/converge-code-changes.git \
  ~/.codex/skills/converge-code-changes
```

开始一个新的 Codex 任务后，技能即可被自动发现。

仓库中包含：

```text
converge-code-changes/
├── SKILL.md
├── README.md
└── agents/
    └── openai.yaml
```

## 使用方式

技能已允许自动调用。以下任务通常会触发它：

- 修复 Bug，尤其是已经经历过一次或多次失败修改时；
- “不要这个方案，换一种实现”；
- “删除、去掉、改回、重做这个功能”；
- “清理刚才的补丁，但保留现在正确的效果”；
- 检查 fallback、兼容层、临时代码或历史注释；
- 删除内容后清理残留布局和样式。

也可以显式调用：

```text
$converge-code-changes 修复这个 Bug，并把之前几轮修改收敛成唯一实现。
```

```text
$converge-code-changes 保留当前正确行为，清除这次修改留下的补丁结构和无意义注释。
```

## 典型场景

### 1. 用户否决了早先方案

错误结果：

```ts
// Do not use the titanium implementation requested earlier.
applyFabricPatch();
```

收敛结果：

```ts
applyFabricPatch();
```

“为什么没有选择钛合金”属于对话历史，不属于当前产品状态。

### 2. 清理补丁，但保留最终行为

当用户说“清理这个补丁”时，技能会区分：

- **正确行为**：用户最终确认需要的结果；
- **补丁残留**：旧分支、wrapper、fallback、过程注释和临时命名。

默认清理第二类，不会把第一类一起删除。

### 3. 删除 UI 内容

删除一个页面模块不只是删除 DOM 或组件。技能还会检查由该模块拥有的：

- 分隔线和边框；
- `min-height`、margin 和空白区域；
- 伪元素、角标色和光晕；
- 事件处理、资源、文案和测试。

这样可以避免“内容已经删了，页面上还留着一条横线”的残余状态。

### 4. Bug 被 fallback 掩盖

如果上游错误地产生 `undefined`，技能不会默认接受：

```ts
const items = result?.items ?? [];
```

它会先追踪数据的 producer、transformer 和 consumer，判断应该在哪个位置恢复真正的契约。只有输入本来就允许缺失时，fallback 才是当前实现的一部分。

## 核心判断标准

仓库最终应该能独立回答“系统现在是什么”，而不是记录“Agent 和用户刚才讨论过什么”。

对每一条注释、分支、兼容层或测试，可以问：

> 一个从未看过对话、只拿到当前仓库的工程师，是否仍然需要这条信息？

如果不需要，它通常不应该留在最终实现里。

## 边界

- 不会借清理之名修改与当前任务无关的既有代码。
- 不会在证据不足时删除真实存在的外部兼容约束。
- 不会把所有 defensive programming 都判为错误；有当前契约依据的防护仍应保留。
- 当“删除行为”和“只删除实现方式”会产生不同用户结果且无法从仓库判断时，Agent 应先提出一个聚焦问题。

## 文件

- [`SKILL.md`](./SKILL.md)：Agent 执行规则。
- [`agents/openai.yaml`](./agents/openai.yaml)：Codex 展示信息和自动调用策略。
