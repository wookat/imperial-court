# Imperial Court（朝廷）— 目标导向智能体的宪政 Skill 套件

> 结构：皇帝（用户）— 宰相（主智能体）— 大臣（子智能体，可递归）。
> 原则：先签契约再放权；宰相可封驳；一切沟通尽量用选择题；一切裁决留痕。

这是「阶段 0」验证套件：零平台代码，纯 Skill + 宪法文档，可运行在 Devin / Claude Code
等支持 Agent Skills（`.agents/skills/*/SKILL.md`）的平台上。目的是用 2–4 周真实使用，
验证两个核心假设：

1. **封驳被需要吗？** —— 统计封驳触发次数与皇帝采纳率；
2. **选择题撑得住吗？** —— 统计用户点选「其他/自定义」的比例（目标 < 40%）。

## 仓库结构

```
memory/constitution.md          # 宪法：约束(C)/目标(G)/方法论(M)/行为论(P) 四章编号条款
memory/decree_log.md            # 诏令与封驳留痕（判例库）
.agents/skills/
  fengbo/SKILL.md               # 封驳：指令过宪审查，冲突则引条款上奏
  contract-narrowing/SKILL.md   # 契约收窄：委派子会话时宪法只收不放
  conduct-protocol/SKILL.md     # 行为论：何时必须上奏 vs 自主决定 + 选择题交互规约
docs/metrics.md                 # 阶段 0 度量口径与杀死条件
```

## 与 github/spec-kit 的关系

本套件与 [spec-kit](https://github.com/github/spec-kit) 互补而非竞争：

- spec-kit 负责**开工前**：`/speckit.constitution` 生成项目宪法、`/speckit.clarify`
  用 A/B/C 选项表澄清需求、`/speckit.specify → plan → tasks` 逐级审批；
- 本套件负责**放权后**：封驳（执行前替皇帝把关指令质量）、契约收窄（委派治理）、
  行为论（执行中的请示纪律）。

已用 spec-kit 的项目：把 `.specify/memory/constitution.md` 作为宪法正文，本仓库的
`memory/constitution.md` 只保留行为论/约束增补章节，条款编号沿用引用即可。

## 用法

1. 把 `memory/` 与 `.agents/skills/` 拷入（或子模块引入）目标项目；
2. 和智能体一起用选择题方式填写 `memory/constitution.md`（首次约 10 分钟）；
3. 正常派任务。宰相会：接令先过宪审查 → 冲突则上奏（引条款 + 替代方案，选择题裁决）→
   执行中按行为论决定自主/上奏 → 委派子会话时下传收窄后的契约 → 全程写 `decree_log.md`；
4. 一个月后按 `docs/metrics.md` 结算数据，决定是否进入阶段 1（写平台代码补强制力）。

## 已知局限（刻意保留到阶段 1 解决）

- 宪法是 prompt 级软约束，高危红线仍依赖宿主平台的权限审批兜底；
- decree_log 靠模型自觉记录，非不可篡改审计；
- 选择题渲染受宿主 UI（user_question / AskUserQuestion）限制；
- 子会话递归深度与预算无硬性闸门。
