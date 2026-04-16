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

系统必须存在唯一控制入口（authority / daemon / service / host）。
→ 不允许并行控制路径、影子入口或绕过 authority 的 mutation

---

## B3. API 是产品语义承诺（Semantic API → Public Promise）

API 必须表达用户意图与产品能力，而不是底层对象图。
→ public surface 是可冻结的承诺（promise），不是内部拼装层的投影

---

## B4. 自动化是一等公民（Automation-first）

所有能力必须可以被 CLI、API 或程序化 workflow 调用。
→ UI 只是视图（view），不是能力入口，也不是状态来源

---

## B5. 系统本质是收敛机（State Convergence → Truth Convergence）

系统设计的核心不是接口集合，而是让意图、运行事实、持久化真相、对外可见状态收敛到同一个事实源。
→ 一次 mutation 应只有一个 commit 点；public state / event / error 都必须从 committed truth 派生

---

## B6. 架构通过删除收敛（Design by Elimination）

系统不是“构建（build）”出来的，而是“删除（eliminate）”出来的。
→ 最优结构 = 最小不可再约简（irreducible structure）

---

## B7. 约束必须机械化（Mechanized Guardrails）

真正重要的架构规则不能只停留在文档或口头约定里。
→ 必须下沉为类型边界、编译约束、测试、guardrail、promise artifact 或 release check

---

# 🧱 2. Constraints（约束）

## C1. UI 不拥有状态

所有状态变更必须经过核心控制层（authority / daemon / service）。
→ 防止状态分裂（state divergence）

---

## C2. 不允许多入口控制

任何 mutation 只能通过单一 authority 进入。
→ 防止不可控行为（uncontrolled behavior）与多路径真相（multiple truths）

---

## C3. 不暴露底层实现

禁止 public API 直接暴露底层类型、框架、传输、存储布局或内部 target。
→ 保持语义边界（semantic boundary）与产品承诺边界（promise boundary）

---

## C4. 所有能力必须可自动化

如果一个能力不能被 CLI / API / workflow 调用
→ 视为未完成（incomplete feature）

---

## C5. Public Promise 必须精确可验证

冻结面不能靠“看起来合理”的文档维护。
→ public symbol、negative promise、growth policy 必须能被机器检查

---

## C6. 完成声明必须有证据

任何 PASS / done / shipped 声明都必须绑定测试、日志、artifact 或可复现命令输出。
→ 没有证据路径的完成声明视为未完成

---

# ⚙️ 3. Decision Rules（决策规则）

### R1. 优先建模能力，而不是结构

IF 设计 API
→ 从用户动作（user action）出发，而不是系统组件

---

### R2. 默认拒绝新增结构

IF 新增模块 / 抽象 / public surface
→ 先尝试合并（merge）、降级（internal / advanced / adapter）或删除（remove）

---

### R3. 所有设计必须可绕过 UI

IF 功能依赖 UI
→ 必须提供 CLI / API 等价路径

---

### R4. 抽象必须减少复杂度

IF 抽象没有减少调用路径、承诺面积或认知成本
→ 删除（remove）

---

### R5. 未发布前优先硬切，不做兼容幻觉

IF 系统尚未正式发布，且当前模型方向错误
→ 选择 hard cut / model replacement，而不是 deprecated 双栈、legacy alias 或补丁式修补

---

### R6. Public API 先问“是否值得冻结”

IF 一个能力进入 public surface
→ 必须判断它是稳定产品承诺，还是 internal / adapter / preview / UI integration

---

### R7. 重要规则必须变成 guardrail

IF 一个规则反复靠人记住
→ 把它下沉为测试、脚本、契约、promise artifact 或编译边界

---

### R8. Mutation 先定义 commit，再定义观察

IF 设计状态变更流程
→ 先定义唯一 commit truth；再从它派生 public snapshot、event、error 与 audit log

---

# ❌ 4. Anti-Patterns（反模式）

* 多控制入口（multiple authority）
* UI 直接控制核心状态
* API 暴露底层实现、框架、存储路径或 transport
* public API 只是内部类型的 typealias / thin wrapper
* “新模型外壳 + 旧路径内核”并存
* stringly-typed `kind + optional fields` 大包结构冒充强契约
* 假开放、假承诺、假边界（fake openness / promise / boundary）
* 隐藏默认值、silent success、fallback 成功、补读拼装状态
* deprecated 双栈 / legacy alias 在未发布系统中长期存在
* guardrail 只冻结当前偶然形态，而不是正确方向
* PASS / done 没有可复现证据
* 为未来假设设计（premature abstraction / fake future）

---

# ⚠️ 5. Tensions（内在张力）

## T1. 控制力 vs 灵活性

Single authority 会压制扩展自由度，但换来可证明的一致性。

---

## T2. 精简 vs 可扩展性

持续删除可能削弱未来扩展能力；但过早保留扩展口会制造假未来。

---

## T3. 语义纯度 vs 实用性

语义 API 与强类型契约可能增加实现成本，但能降低长期承诺债务。

---

## T4. 硬切 vs 兼容

未发布阶段倾向 hard cut；发布后必须尊重真实用户承诺。

---

## T5. 机械约束 vs 迭代速度

guardrail / promise artifact 会增加短期成本，但能防止系统回滑。

---

## T6. 单一真相 vs 观察便利性

observer、event、error projection 越方便，越容易反向拖住或污染内核。

---

# 🔥 6. Destructive Thinking（破坏性思维）

默认检查：

* UI 崩溃 → 系统是否仍然运行？
* 是否存在绕过 authority 的 mutation 路径？
* 一次 mutation 是否只有一个 commit 点？
* public state 是否只从 committed snapshot 推导？
* event 是否发生在 commit 之后？
* observer / callback 是否可能反压内核？
* error projection 是否只是边界翻译，而不是内部真相来源？
* public API 是否承诺了真实产品能力之外的实现细节？
* guardrail 是否锁住“正确方向”，还是只锁住“当前偶然形态”？
* 是否存在重复语义、影子状态、补读拼装、隐藏默认值？
* PASS / done 是否有测试、日志、artifact 或命令输出证明？

---

# 🪶 7. Simplification Log（精简日志）

## Init

* 将设计哲学压缩为 6 个核心信念
* 删除重复表达与细节噪音

---

## 2026 Sessions Pass

* 将 “状态机” 抽象提升为 “truth convergence / single commit truth”
* 将反复出现的 guardrail、public promise、negative promise 合并为 “mechanized guardrails”
* 将 GhostVM 具体细节压缩为可迁移规则：public promise、semantic kernel、hard cut、single truth
* 删除对具体 target / 文件名 / beta 轮次的依赖，保留设计偏好而非项目历史

---

# 🧬 8. Evolution Log（演化）

## Phase 1

* 明确 single authority
* 强化 automation-first
* 引入 design by elimination

---

## Phase 2（当前）

* 从 “Semantic API” 演化为 “Public Promise”：public surface 必须值得冻结且可验证
* 从 “State Machine” 演化为 “Truth Convergence”：commit / snapshot / event / error 必须同源
* 从 “文档约束” 演化为 “机械约束”：guardrail、promise artifact、release check 成为架构的一部分
* 从 “持续精简” 演化为 “删掉假未来”：删除不真实的扩展口、兼容层、隐藏承诺

---

# 📊 9. Active Focus（当前关注）

* public promise 是否过宽，是否包含未准备冻结的能力
* negative promise 是否能阻止已删除语义回归
* guardrail 是否验证正确方向，而非冻结当前偶然实现
* mutation 是否有唯一 commit truth，观察链路是否只读派生
* mechanical refactor 是否严格保持行为；semantic hard cut 是否明确破坏边界
* 是否存在未验证的抽象、假未来、假开放或文档型约束

---

# 🎯 10. Meta Insight（元认知）

> 架构的本质不是组织代码，而是控制复杂度（control complexity）与控制真相（control truth）。

> 最好的系统不是功能最多，而是结构最少、承诺最窄、真相路径最短。

> 如果一条规则真的重要，它最终必须变成机器可检查的事实。
