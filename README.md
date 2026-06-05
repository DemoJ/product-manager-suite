# Product Manager Suite

完整的产品经理全流程 Skill，用于帮助人类用户和 AI Agent 从模糊想法、用户反馈、老板指令、竞品功能、数据现象或 PRD 草稿出发，完成需求澄清、方案设计、MVP 拆分、PRD 撰写、指标实验、路线图规划、风险评审和上线复盘。

> **最重要的定位**：这是一个可安装到 Agent 环境中的产品经理能力包。安装后，Agent 在遇到需求、PRD、产品方案、用户反馈、MVP、路线图、指标、A/B 实验等任务时，应读取 `SKILL.md`，并按其中的路由规则使用 `references/`、`templates/` 和 `examples/`。

## 安装说明

### 给人看的安装方式

将本仓库下载或克隆到本地，然后把整个项目目录放到你所使用 Agent 的技能、扩展或上下文目录中。不同 Agent 的目录约定可能不同，请以对应工具的文档为准。

下面以 Claude Code 为例。

#### Windows PowerShell

```powershell
git clone https://github.com/DemoJ/product-manager-suite.git "$env:USERPROFILE\.claude\skills\product-manager-suite"
```

#### macOS / Linux

```bash
git clone https://github.com/DemoJ/product-manager-suite.git ~/.claude/skills/product-manager-suite
```

安装后目录应类似：

```text
<agent-skills-dir>/product-manager-suite/SKILL.md
<agent-skills-dir>/product-manager-suite/references/
<agent-skills-dir>/product-manager-suite/templates/
<agent-skills-dir>/product-manager-suite/examples/
```

如果使用 Claude Code，安装后重启 Claude Code，或开启一个新的 Claude Code 会话。

如果你不是通过 Git 安装，也可以直接下载本仓库 ZIP，解压后把文件夹重命名为 `product-manager-suite`，再放入对应 Agent 的技能、扩展或上下文目录。

### 给 Agent 看的安装提示词

```text
请从这个仓库安装skill并使用： https://github.com/DemoJ/product-manager-suite
```

## 如何验证安装成功

安装后，用以下任一提示测试：

```text
我有个想法：做一个给销售团队用的客户跟进提醒工具。帮我判断是否值得做，并拆一个 MVP。
```

期望 Agent 输出应包含：

- 问题定义，而不只是功能列表
- 目标用户、场景、痛点和业务目标
- 关键假设与待确认问题
- 至少 2-3 个备选方案或取舍说明
- 推荐 MVP 范围和暂不做范围
- 成功指标、风险、验证方式和下一步行动

如果 Agent 只是直接写功能清单，没有问题定义、假设、指标和风险，说明它没有正确使用本 Skill。

## 适用场景

当用户需要处理以下产品工作时，可以使用本 Skill：

- 需求澄清与问题定义
- 用户、场景、痛点和业务目标分析
- 需求评审与 PRD 审查
- 头脑风暴与方案发散
- 产品方案设计与 MVP 拆分
- PRD、用户故事、验收标准和业务规则撰写
- 多需求优先级排序与路线图规划
- 指标、埋点、实验和 A/B 测试设计
- 风险评审、上线检查和回滚预案
- 干系人沟通、汇报材料和会议纪要
- 上线复盘、效果归因和迭代计划

## 核心原则

本 Skill 强调产品经理工作的完整链路，而不是直接把用户给出的方案包装成文档：

1. **先定义问题，再讨论方案**：从用户提出的方案中反推真实问题和业务目标。
2. **先识别用户和场景，再设计功能**：确保需求落到具体用户、触发场景和期望结果。
3. **区分事实、假设和待确认问题**：避免把缺失信息伪装成确定结论。
4. **发散后必须收敛**：头脑风暴后给出聚类、排序和推荐下一步。
5. **复杂需求先做 MVP**：默认拆出最小可验证版本。
6. **方案必须可验证**：每个方案都应包含风险、指标和验证方式。
7. **输出必须可执行**：给出结论、依据、取舍、下一步行动和确认人。

## 项目结构

```text
.
├── SKILL.md                         # Skill 入口与总体工作原则
├── references/                      # 产品经理工作流参考资料
│   ├── 00-routing.md                # 任务路由
│   ├── 01-product-thinking-principles.md
│   ├── 02-requirement-analysis.md
│   ├── 03-brainstorming.md
│   ├── 04-solution-design.md
│   ├── 05-prd-writing.md
│   ├── 06-prioritization-and-roadmap.md
│   ├── 07-metrics-and-experiments.md
│   ├── 08-review-and-risk.md
│   ├── 09-stakeholder-communication.md
│   └── 10-retrospective-and-iteration.md
├── templates/                       # 标准化输出模板
│   ├── requirement-review-report.md
│   ├── brainstorm-report.md
│   ├── solution-proposal.md
│   ├── prd-standard.md
│   ├── prd-lite.md
│   ├── roadmap.md
│   ├── experiment-plan.md
│   ├── stakeholder-brief.md
│   └── retrospective.md
├── examples/                        # 使用示例
│   ├── vague-idea-to-product-plan.md
│   ├── user-feedback-to-solution.md
│   ├── boss-request-to-solution.md
│   ├── prd-review-example.md
│   └── brainstorm-example.md
└── evals/
    └── evals.json                   # Skill 评估用例
```

## 工作流概览

当用户没有明确指定产出格式时，默认按以下顺序推进：

1. **理解输入**：提取事实、目标、约束、已有方案和缺失信息。
2. **问题澄清**：识别真实问题、目标用户、使用场景和业务目标。
3. **需求分析**：评估用户价值、商业价值、可行性、风险和优先级。
4. **方案发散**：从低成本优化、体验增强、自动化/智能化、运营策略、商业化等方向生成方案。
5. **方案收敛**：对比价值、成本、风险、速度和验证难度，推荐主方案。
6. **MVP / PRD 化**：拆出本期范围、暂不包含范围、用户故事、业务规则和验收标准。
7. **指标与风险**：设计成功指标、护栏指标、埋点、实验或上线验证方式。
8. **下一步行动**：列出需要确认的问题、协作对象、近期行动和决策点。

## 输出类型

根据任务类型，Skill 会选择合适的输出模式：

| 输出模式 | 适用场景 |
|---|---|
| 快速判断 | 对需求或方案做快速可行性判断 |
| 需求审查报告 | 已有需求、PRD、截图或文档需要评审 |
| 头脑风暴报告 | 需要发散创意、方案或增长方向 |
| 产品解决方案 | 需要从问题推导到推荐方案 |
| PRD | 需求进入研发、设计和测试协作阶段 |
| 路线图 | 多需求排序、版本规划和资源取舍 |
| 实验设计 | 需要验证功能效果、指标或策略假设 |
| 复盘报告 | 上线后效果归因和迭代计划 |

## 示例请求

```text
帮我把这个模糊想法整理成一个产品方案。
```

```text
请评审这份 PRD，指出问题、风险和缺失信息。
```

```text
基于这批用户反馈，帮我提炼核心需求并设计 MVP。
```

```text
帮我为这个新功能设计指标、埋点和 A/B 实验。
```

## 维护建议

- 新增产品方法论时，优先放入 `references/`。
- 新增可复用交付物格式时，优先放入 `templates/`。
- 新增典型用户输入与输出样例时，放入 `examples/`。
- 修改 Skill 触发、路由和总体原则时，更新 `SKILL.md`。
- 若新增或调整关键能力，建议同步更新 `evals/evals.json`。
