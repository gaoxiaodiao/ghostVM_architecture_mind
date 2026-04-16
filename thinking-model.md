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

系统必须存在唯一控制入口（authority / daemon / service / host / runner）。
→ 不允许并行控制路径、影子入口或绕过 authority 的 mutation

---

## B3. API 是产品语义承诺（Semantic API → Public Promise）

API 必须表达用户意图与产品能力，而不是底层对象图。
→ public surface 是可冻结的承诺（promise），不是内部拼装层的投影

---

## B4. 自动化必须共享语义入口（Automation-first → Unified Semantic Entry）

App、CLI、AppleScript/API、E2E automation 不应各自实现能力。
→ UI / CLI / automation 都应调用同一 semantic command / domain action / executor path

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

## B8. 执行链也是架构（Workflow as Contract System）

Agent workflow、build/release、runbook、storage placement、auth identity、liveness policy 都会改变系统真相。
→ 执行必须有稳定身份、明确阶段、证据 artifact 与可恢复的交接路径

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

## C4. 自动化路径必须保持产品语义

如果一个能力不能被 CLI / API / workflow 调用，视为未完成。
如果自动化路径验证的是另一套行为，也视为未完成。

---

## C5. Public Promise 必须精确可验证

冻结面不能靠“看起来合理”的文档维护。
→ public symbol、negative promise、growth policy 必须能被机器检查

---

## C6. 完成声明必须有运行证据

任何 PASS / done / shipped 声明都必须绑定测试、日志、artifact、截图、真实运行状态或可复现命令输出。
→ 静态配置存在、文件存在、命令返回成功、模型口头确认都不足以证明产品行为

---

## C7. 执行身份与阶段必须可追溯

任务必须有稳定 task_id / run_id / step 边界；runner / auth / env / timeout / termination 都是架构状态。
→ 防止无法恢复、无法审计、无法复现的 agent 或 release workflow

---

## C8. 持久化位置是系统契约

高写入状态、日志、配置、镜像、运行脚本不能隐式落在默认脆弱位置。
→ state path 必须可说明、可验证、可迁移、可交接

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
→ 必须提供 CLI / API 等价路径，并尽量共享同一 command semantics

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

### R9. 验证必须贴近用户真实环境

IF 验收目标是用户可见行为
→ 优先使用真实 app / OS gate / permission / window / binary / runtime artifact；synthetic harness 必须证明等价

---

### R10. 不确定平台边界先 POC

IF OS / framework / cross-process / UI ownership 存在未知限制
→ 先做最小 POC 或真实探针，再冻结架构

---

### R11. 高风险执行先分阶段

IF workflow 涉及持久化基础设施、发布、签名、agent 编排或数据迁移
→ 先定义 phase、authority、证据、回滚/恢复边界，再执行 mutation

---

# ❌ 4. Anti-Patterns（反模式）

* 多控制入口（multiple authority）
* UI 直接控制核心状态
* App / CLI / AppleScript / E2E 各自实现同一能力
* API 暴露底层实现、框架、存储路径或 transport
* public API 只是内部类型的 typealias / thin wrapper
* “新模型外壳 + 旧路径内核”并存
* stringly-typed `kind + optional fields` 大包结构冒充强契约
* 假开放、假承诺、假边界（fake openness / promise / boundary）
* fake verification：只检查文件存在、mock 成功、prose 生成或命令退出码
* synthetic E2E harness 与真实产品运行环境不一致
* 隐藏默认值、silent success、fallback 成功、补读拼装状态
* deprecated 双栈 / legacy alias 在未发布系统中长期存在
* guardrail 只冻结当前偶然形态，而不是正确方向
* docs 是散乱笔记，而不是 agent / human 可恢复的操作记忆
* orchestrator 与目标仓库逻辑耦合，或 controller 递归调度 controller
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

## T7. 真实验收 vs 测试速度

真实 app / OS / permission / UI / runtime gate 更能证明产品行为，但更慢、更脆弱、更依赖环境。

---

## T8. Agent 自主性 vs 设计检查点

默认希望 agent 持续执行到终态；但基础设施、发布、数据迁移等高风险工作必须先有设计与验收边界。

---

## T9. 平台真实约束 vs 架构纯度

OS / framework / signing / TCC / process ownership 可能打破理想分层；架构必须承认真实物理约束。

---

# 🔥 6. Destructive Thinking（破坏性思维）

默认检查：

* UI 崩溃 → 系统是否仍然运行？
* 是否存在绕过 authority 的 mutation 路径？
* App / CLI / AppleScript / E2E 是否共享同一语义入口？
* 一次 mutation 是否只有一个 commit 点？
* public state 是否只从 committed snapshot 推导？
* event 是否发生在 commit 之后？
* observer / callback 是否可能反压内核？
* error projection 是否只是边界翻译，而不是内部真相来源？
* public API 是否承诺了真实产品能力之外的实现细节？
* 验证是否运行在真实用户环境，而不是自造 harness？
* state path / auth identity / version source / build phase 是否显式？
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

## Daily Review Pass（2026-01-01 → 2026-04-17）

* 按 107 个日 step 复查 752 个 session 文件，明确 38 个无 session 日
* 将 January 的 agent-contract / workflow-state 信号压缩为 B8 / C7，而不是展开成编排手册
* 将 February 的 durable storage / runbook / AI-readable docs 信号压缩为 C8 与 docs-as-operational-memory 反模式
* 将 March 的 convergence / public promise / hard cut 信号作为既有核心的强化，不再新增 GhostVM 专属规则
* 将 April 的 unified semantic entry / real E2E / platform honesty 信号压入 B4、R9、R10 与 T7–T9

---

# 🧬 8. Evolution Log（演化）

## Phase 1

* 明确 single authority
* 强化 automation-first
* 引入 design by elimination

---

## Phase 2

* 从 “Semantic API” 演化为 “Public Promise”：public surface 必须值得冻结且可验证
* 从 “State Machine” 演化为 “Truth Convergence”：commit / snapshot / event / error 必须同源
* 从 “文档约束” 演化为 “机械约束”：guardrail、promise artifact、release check 成为架构的一部分
* 从 “持续精简” 演化为 “删掉假未来”：删除不真实的扩展口、兼容层、隐藏承诺

---

## Phase 3（当前）

* 从 “可自动化” 演化为 “统一语义入口”：automation 必须验证同一套产品行为
* 从 “完成有证据” 演化为 “真实运行证据”：真实 app / OS / artifact / permission / runtime gate 优先
* 从 “代码架构” 扩展为 “执行链架构”：task identity、runner auth、phase、liveness、handoff docs 都是系统边界
* 从 “文档记录” 演化为 “agent interface”：文档必须帮助未来 agent / human 安全恢复上下文

---

# 📊 9. Active Focus（当前关注）

* public promise 是否过宽，是否包含未准备冻结的能力
* App / CLI / AppleScript / E2E 是否共享一条 semantic command path
* E2E 证据来自真实 app/runtime，还是来自偏离产品的 synthetic harness
* negative promise 是否能阻止已删除语义回归
* guardrail 是否验证正确方向，而非冻结当前偶然实现
* mutation 是否有唯一 commit truth，观察链路是否只读派生
* state path、version source、runner identity、phase boundary 是否显式
* mechanical refactor 是否严格保持行为；semantic hard cut 是否明确破坏边界
* 是否存在未验证的抽象、假未来、假开放或文档型约束

---

# 🎯 10. Meta Insight（元认知）

> 架构的本质不是组织代码，而是控制复杂度（control complexity）与控制真相（control truth）。

> 最好的系统不是功能最多，而是结构最少、承诺最窄、真相路径最短。

> 自动化不是旁路；它必须证明与用户实际使用的是同一个系统。

> 如果一条规则真的重要，它最终必须变成机器可检查的事实。
