# Global HR Compliance Playbook — 架构文档

**版本**: V1.0 | **更新日期**: 2026-05-31

---

## 概述

Global HR Compliance Playbook 是一个面向 Claude Code 的原子化 Skill，用于生成全球各国/地区的用工合规手册。

系统将职责拆分为独立的原子化文件，通过主入口 `skill.md` 以 `#[[file:路径]]` 语法组装成完整 skill。

---

## 目录结构

```text
global-hr-compliance-playbook/
│
├── skill.md                        # 主入口（组装所有模块）
├── README.md                       # 使用说明（English）
├── README.zh-CN.md                 # 使用说明（中文）
├── ARCHITECTURE.md                 # 本文档
├── CHANGELOG.md                    # 版本更新日志
│
├── 1_core/                         # 角色与核心定义
│   ├── identity.md                 # 专家角色定义
│   ├── capabilities.md             # 四大核心能力
│   └── output_principles.md        # 输出标准与护栏
│
├── 2_tools/                        # 工具使用规范
│   ├── web_tools.md                # 网络搜索与内容抓取
│   └── usage_principles.md         # 工具使用原则
│
├── 3_workflow/                     # 研究与内容生产工作流
│   ├── scope_confirmation.md       # 地域范围 + 用户画像确认
│   ├── scope_confirmation_checklist.md  # 确认清单模板
│   ├── research_process.md         # 三阶段研究方法
│   ├── research_plan_template.md   # 研究计划模板
│   ├── deep_research.md            # 深度研究要求
│   ├── key_questions.md            # 必答问题清单
│   ├── research_to_chapter_mapping.md   # 研究关键词→章节映射
│   ├── info_distribution_guide.md  # 信息分配指南
│   ├── detail_supplement_plan.md   # 细节补充计划
│   ├── chapter_output_protocol.md  # 章节输出协议
│   ├── quality_validation.md       # 质量验证（输出前 spec 对照 + 三阶段 pass）
│   ├── source_standards.md         # 信息来源层级规则
│   ├── module_relations.md         # 章节间交叉引用关系
│   ├── reference_docs_guide.md     # 参考文档使用指南
│   └── incremental_update.md       # 增量更新机制
│
├── 4_output/                       # 输出规范
│   ├── structure.md                # 手册结构索引
│   ├── segmented_strategy.md       # 分批输出策略
│   ├── content_layers.md           # 三层内容模型
│   ├── legal_updates.md            # 法律更新预警协议
│   ├── format_template.md          # 14 章格式骨架模板
│   ├── terminology_standards.md    # 术语与表述标准
│   └── chapter_scale_guide.md      # 章节规模指引
│
├── 5_spec/                         # 内容规格与模板
│   ├── handbook_structure.md       # 手册固定章节结构
│   ├── format_spec.md              # 格式规格（Markdown 写作规范）
│   ├── content_layers_spec.md      # 三层内容规格
│   ├── mandatory_questions.md      # 必答问题
│   ├── naming_convention.md        # 输出文件命名规范 + 版本管理
│   └── chapters/                   # 各章节规格文件（14 个）
│       ├── ch01_preface.md
│       ├── ch02_country_overview.md
│       ├── ch03_recruitment.md
│       ├── ch04_compensation.md
│       ├── ch05_tax.md
│       ├── ch06_social_security.md
│       ├── ch07_contract.md
│       ├── ch08_leave.md
│       ├── ch09_union_handbook.md
│       ├── ch10_data_compliance.md
│       ├── ch11_labor_dispute.md
│       ├── ch12_forex.md
│       ├── ch13_risk_warning.md
│       └── ch14_resources.md
│
└── 6_meta/                         # 元信息层
    └── notes.md                    # 关键约束与注意事项
```

**文件统计**: 6 个目录，47 个内容文件，纯配置无生成产物。

---

## 模块职责

### 1_core — 角色层

定义 AI 的专家身份与行为边界。

| 文件 | 职责 |
|:-----|:-----|
| `identity.md` | 全球 HR 合规专家角色，覆盖招聘→在职→离职全周期 |
| `capabilities.md` | 四大能力：全球 HR 合规、跨司法管辖劳动法、外籍员工雇用、数据隐私 |
| `output_principles.md` | 输出护栏：规则层级分类、风险披露、信息降级策略 |

### 2_tools — 工具层

规范外部工具的使用优先级与验证要求。

| 文件 | 职责 |
|:-----|:-----|
| `web_tools.md` | 官方政府网站优先；关键数据交叉验证；记录来源与时间戳 |
| `usage_principles.md` | 官方来源优先 → 交叉核实 → 来源归因 → 降级处理 |

### 3_workflow — 工作流层

从用户请求到最终输出的完整执行流程。

| 文件 | 职责 | 阶段 |
|:-----|:-----|:-----|
| `scope_confirmation.md` | 地域确认 + 用户画像 | 启动 |
| `scope_confirmation_checklist.md` | 确认清单模板 | 启动 |
| `research_process.md` | 三阶段研究方法 | 研究 |
| `research_plan_template.md` | 研究计划模板 | 研究 |
| `deep_research.md` | 深度研究要求 | 研究 |
| `key_questions.md` | 必答问题清单 | 研究 |
| `research_to_chapter_mapping.md` | 研究关键词→章节映射 | 研究 |
| `info_distribution_guide.md` | 信息按章节分配 | 整理 |
| `detail_supplement_plan.md` | 识别缺口，定向搜索 | 补全 |
| `chapter_output_protocol.md` | 章节输出协议 | 输出 |
| `quality_validation.md` | 输出前 spec 对照检查 + 结构/内容/实务三阶段 pass | 校验 |
| `source_standards.md` | 信息来源层级规则 | 全程 |
| `module_relations.md` | 章节间交叉引用 | 全程 |
| `reference_docs_guide.md` | 参考文档使用指南 | 全程 |
| `incremental_update.md` | 增量更新流程与交叉引用表 | 更新 |

### 4_output — 输出层

定义最终交付物的格式、结构与质量标准。

| 文件 | 职责 |
|:-----|:-----|
| `structure.md` | 手册结构索引（引用 5_spec） |
| `segmented_strategy.md` | 分批连续输出，支持断点续传 |
| `content_layers.md` | 三层模型：规则层 / 实务层 / 风险层 |
| `legal_updates.md` | 法律更新预警协议 |
| `format_template.md` | 14 章标题骨架模板，禁止从零构建 |
| `terminology_standards.md` | 禁用词表 + 图例标准 + 术语一致性 |
| `chapter_scale_guide.md` | 各章目标行数 + "准且特殊"原则 |

### 5_spec — 规格层

每章内容必须满足的详细规格定义，是质量保障的核心标准。

14 个章节规格文件 + 5 个通用规格文件，定义了手册结构、格式、内容深度、必答问题。

| 文件 | 职责 |
|:-----|:-----|
| `handbook_structure.md` | 手册 14 章固定结构（章节编号唯一来源） |
| `format_spec.md` | 飞书 Markdown 格式规格 |
| `content_layers_spec.md` | 三层内容规格 |
| `mandatory_questions.md` | 必答问题 |
| `naming_convention.md` | 输出文件命名 + 版本管理 |
| `chapters/ch[01-14]_*.md` | 14 章逐章详细规格（按需读取，不预加载） |

### 6_meta — 元信息层

| 文件 | 职责 |
|:-----|:-----|
| `notes.md` | 关键约束与注意事项 |

---

## 数据流

```text
用户请求（目标国家/地区）
        │
        ▼
[ Mode dispatch ]
完整手册 / 法规快查 / 实务咨询 / 增量更新 → 走对应分支
        │
        ▼  （以下为完整手册流程）
┌─────────────────────────┐
│  1. Scope 确认           │  scope_confirmation.md
│  - 地域范围              │
│  - 用户画像（受众）      │
│  - 已有材料确认          │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  2. 创建草稿文件         │  [国家]_draft.md（skill 同目录）
│  + 基于模板骨架初始化    │  format_template.md
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  3. 三阶段研究           │  research_process.md
│  Phase 1: 官方法规       │
│  Phase 2: 权威解读       │
│  Phase 3: 定向补全       │
│  → 每个模块完成后写入草稿│
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  4. 信息分发             │  info_distribution_guide.md
│  按 14 章归类，标注缺口  │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  5. 逐模块深化           │  detail_supplement_plan.md
│  对照 spec 补全缺口      │  5_spec/chapters/ch*.md
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  6. 输出前 spec 对照检查 │  quality_validation.md（前置环节）
│  逐章读 spec → 对比草稿  │
│  → 补全缺口 → 再输出     │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  7. 分批连续输出         │  segmented_strategy.md
│  5 批 × 2-3 章/批        │
│  → 写入最终文件          │
└──────────┬──────────────┘
           │
           ▼
┌─────────────────────────┐
│  8. 质量验证三阶段       │  quality_validation.md
│  Pass 1: 结构检查        │  - 标题/编号/禁用词
│  Pass 2: 内容检查        │  - 缺口/重复/链接
│  Pass 3: 实务检查        │  - 可操作性/信息密度
└──────────┬──────────────┘
           │
           ▼
  [国家]用工合规手册_YYYYMMDD_Vx.x.md
```

---

## 关键设计原则

### 1. 关注点分离

角色定义（1_core）/ 工具使用（2_tools）/ 执行流程（3_workflow）/ 输出格式（4_output）/ 内容标准（5_spec）各自独立，互不耦合。修改某一层不影响其他层。

### 2. 规格驱动质量

每章内容必须对照 `5_spec/chapters/ch[XX]_*.md` 规格文件校验。"输出前 spec 对照检查"为强制步骤，不可跳过。

### 3. 三层内容模型

每个主题模块必须覆盖：
- **规则层** — 法律规定什么（条款、数字、期限）
- **实务层** — 企业如何执行（流程、步骤、检查清单）
- **风险层** — 常见问题与预防（处罚、案例、应对）

### 4. "准且特殊"优先

该国特有的规则要深写，通用知识一笔带过。每个段落必须包含具体数字、操作步骤、法条引用或风险场景中的至少一项。

### 5. 来源优先级

```text
官方政府网站、官方法律文本
        ↓
政府指南、四大会计师事务所、知名律所
        ↓
其他权威解读
        ↓（无来源时）
⚠️ 待核实 标注降级
```

### 6. 格式硬约束

必须基于 `format_template.md` 的骨架模板生成，禁止从零构建标题结构。术语必须遵循 `terminology_standards.md` 的禁用词表和图例标准。

### 7. 增量更新支持

通过 `incremental_update.md` 的交叉引用表，法律变更时自动识别所有关联章节，输出 diff 格式变更而非重写全文。

### 8. Mode dispatch 分级响应

简单请求（法规快查、实务咨询）不走 8 步重型流程，仅触发"搜索 + 简明回答"。完整手册生成才进入完整 workflow。

---

## 约束与护栏

| 约束 | 说明 |
|:-----|:-----|
| 步骤不可跳过 | 工作流各步骤顺序执行，"输出前 spec 对照检查"为强制步骤 |
| 来源必须可追溯 | 所有事实必须注明来源，无来源必须标注 ⚠️ |
| 14 章 + 必答问题 | 任何手册都必须完整覆盖，缺一不可 |
| 格式严格 | 必须基于模板骨架，禁用词表强制执行 |
| 特殊地区须确认 | 存在法律差异时，必须先与用户确认管辖范围 |
| 三层内容必须 | 每个章节必须包含规则层 + 实务层 + 风险层 |
| 草稿实时落盘 | 研究内容必须写入草稿文件，不得只存在于对话 context |
| 术语一致性 | 全文不得出现禁用词（如 HRBP），输出前 grep 检查 |
| 路径相对化 | 草稿与最终文件均保存在 skill 同目录，不写死绝对路径 |

---

## 入口与激活

**主入口**: `skill.md`

**激活方式**:
- 显式调用: `/global-hr-compliance-playbook [参数]`
- 隐式触发: 对话中提及生成合规手册、查询劳动法、解答 HR 合规问题

**输出文件命名**: `[国家名]用工合规手册_YYYYMMDD_Vx.x.md`

**输出保存位置**: skill 同目录（即与 `skill.md` 同级，相对路径，便于 skill 重命名 / 迁移）
