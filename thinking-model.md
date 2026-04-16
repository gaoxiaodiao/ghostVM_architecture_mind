# 🧠 Design Philosophy Model

> A continuously evolving model of my software design philosophy.
> Built from real decisions, refined through elimination.

---

# 🧬 1. Core Beliefs（核心信念）

## B1. 架构由用户能力反推（Capability → Architecture）

系统设计必须从“用户能做什么（user capability）”出发，而不是技术结构。
→ 架构是能力的映射（mapping），不是实现的堆叠（implementation stacking）

---

## B2. 控制权必须唯一（Single Authority）

系统必须存在唯一控制入口（daemon / service）。
→ 不允许并行控制路径（parallel control paths）

---

## B3. API 是语义，不是封装（Semantic API）

API 必须表达用户意图（intent），而不是底层实现（implementation）。
→ 不暴露拼装层（assembly layer）

---

## B4. 自动化是一等公民（Automation-first）

所有能力必须可以被 CLI 或程序调用。
→ UI 只是视图（view），不是能力入口

---

## B5. 系统本质是状态机（State Convergence）

系统设计的核心是状态收敛（state convergence），而不是接口集合。
→ 一致性（consistency）优先于灵活性

---

## B6. 架构通过删除收敛（Design by Elimination）

系统不是“构建（build）”出来的，而是“删除（eliminate）”出来的。
→ 最优结构 = 最小不可再约简（irreducible structure）

---

# 🧱 2. Constraints（约束）

## C1. UI 不拥有状态

所有状态变更必须经过核心控制层（daemon/service）
→ 防止状态分裂（state divergence）

---

## C2. 不允许多入口控制

任何功能只能通过单一 authority 进入
→ 防止不可控行为（uncontrolled behavior）

---

## C3. 不暴露底层实现

禁止 API 直接暴露底层类型或框架（如 virtualization.framework）
→ 保持语义边界（semantic boundary）

---

## C4. 所有能力必须可自动化

如果一个能力不能被 CLI 调用
→ 视为未完成（incomplete feature）

---

# ⚙️ 3. Decision Rules（决策规则）

### R1. 优先建模能力，而不是结构

IF 设计 API
→ 从用户动作（user action）出发，而不是系统组件

---

### R2. 默认拒绝新增结构

IF 新增模块 / 抽象
→ 先尝试合并（merge）或复用（reuse）

---

### R3. 所有设计必须可绕过 UI

IF 功能依赖 UI
→ 必须提供 CLI / API 等价路径

---

### R4. 抽象必须减少复杂度

IF 抽象没有减少调用路径或认知成本
→ 删除（remove）

---

# ❌ 4. Anti-Patterns（反模式）

* 多控制入口（multiple authority）
* UI 直接控制核心状态
* API 暴露底层实现
* 抽象只是“包一层”（thin wrapper）
* 为未来假设设计（premature abstraction）

---

# ⚠️ 5. Tensions（内在张力）

## T1. 控制力 vs 灵活性

强控制（single authority）会压制扩展性（flexibility）

---

## T2. 精简 vs 可扩展性

持续删除（simplification）可能削弱未来扩展能力

---

## T3. 语义纯度 vs 实用性

语义 API 可能增加实现复杂度

---

# 🔥 6. Destructive Thinking（破坏性思维）

默认检查：

* UI 崩溃 → 系统是否仍然运行？
* 是否存在绕过 authority 的路径？
* 状态是否可能不一致？
* 是否存在重复语义？

---

# 🪶 7. Simplification Log（精简日志）

## Init

* 将设计哲学压缩为 6 个核心信念
* 删除重复表达与细节噪音

---

# 🧬 8. Evolution Log（演化）

## Phase 1（当前）

* 明确 single authority
* 强化 automation-first
* 引入 design by elimination

---

# 📊 9. Active Focus（当前关注）

* 是否存在过度设计（over-engineering）
* 是否可以进一步精简（simplification）
* 是否存在未验证的抽象（unvalidated abstraction）

---

# 🎯 10. Meta Insight（元认知）

> 架构的本质不是组织代码，而是控制复杂度（control complexity）

> 最好的系统不是功能最多，而是结构最少且完整（minimal yet complete）

