---
name: global-hr-compliance-playbook
description: "Use when users need overseas HR compliance research, country employment compliance handbooks, labor law lookup, global hiring operations advice, or updates to existing HR compliance manuals."
version: "1.0"
allowed-tools:
  - WebSearch
  - WebFetch
  - Read
  - Edit
  - Write
  - Grep
---

# Global HR Compliance Playbook - 全球人力合规手册

## Overview

一个面向海外人力合规与运营场景的 Skill，主要用于生成国家/地区用工合规手册、查询劳动法规细节，覆盖招聘、薪酬税务、社保、劳动合同、签证/工作许可、员工关系、数据合规等员工全生命周期议题。

通过来源优先级控制、关键信息交叉验证、规则层级区分、Spec 对照检查及多层质量复核机制，提升法规检索与内容输出的准确性、权威性和可追溯性，降低 AI 幻觉及信息误用风险。

## When to use

当用户请求与海外用工合规相关时激活，包括但不限于：
- 生成/编写某国家或地区的用工合规手册
- 查询特定国家的劳动法规定（如：签证、加班费、试用期、解雇赔偿）
- 解答具体的海外 HR 合规实务问题（如何解雇、如何派驻、如何裁员）
- 对已有合规手册进行审查、补充或增量更新

## When not to use

不要在以下场景触发或继续使用本 Skill：
- 用户仅要求中国境内 HR 制度、国内劳动法或国内员工关系问题，且不涉及海外用工。
- 用户仅要求翻译、润色、排版或格式转换，不涉及 HR 合规判断。
- 用户要求出具最终法律意见、替代当地律师签字确认，或要求规避当地强制性法律义务。
- 用户提供未脱敏的员工个人敏感信息，且该信息并非完成任务所必需。
- 用户要求基于未经核验的传闻、非权威材料或模型记忆直接输出确定性法规结论。

## Input validation

在输出合规结论前，先确认或合理推定以下输入；缺失时必须说明假设，并给出条件性结论：
- 国家/地区，以及是否涉及特殊司法管辖区（如美国州法、阿联酋 DIFC/ADGM）。
- 员工类型：本地员工、外籍员工、派驻员工、EOR 员工、独立承包商、实习生等。
- 法定雇主、发薪主体、实际工作地点及拟适用合同法律。
- 具体议题：招聘、合同、薪酬、个税、社保、签证/工作许可、解雇、裁员、数据合规等。
- 是否已有内部政策、EOR 协议、当地律师意见或供应商材料。

若问题涉及个案解雇、裁员、签证拒签、税务居民身份、社保豁免、数据跨境或处罚后果，必须提示需 Legal / Tax / Finance / visa vendor / EOR provider / local counsel 复核。

## Error handling

遇到信息不足、来源冲突或工具限制时，按以下规则处理：
- 如果未检索到官方来源，明确写明“未检索到官方来源”，并将结论标记为待 Legal / vendor / official authority 确认。
- 如果搜索工具不可用，不得伪称已检索；可基于模型知识给出初步判断，但必须标注“需人工核实”。
- 如果官方来源与二手解读冲突，以最新有效的官方来源为准；仍无法判断时，列出冲突点和待确认事项。
- 如果不同官方机构口径不一致，优先采用直接主管机构口径；仍有分歧时，不得输出确定性结论。
- 如果关键输入缺失且无法合理推定，先提出必要澄清问题；如需继续输出，必须列明适用假设。

## Permissions and tool use

本 Skill 允许使用 `allowed-tools` 中列明的检索、读取、编辑和写入工具。执行时遵守以下原则：
- 优先使用 WebSearch / WebFetch 获取最新官方法规、政府指南、监管口径和权威解读。
- 使用 Read / Grep 按需读取本 Skill 的规范文件，不预加载无关文件。
- 在完整手册生成模式下，使用 Edit / Write 将研究结果实时写入草稿和最终手册。
- 不写入 API key、密码、员工身份证件号、薪酬明细等敏感信息；处理 HR 数据时应最小化个人信息。

## Mode dispatch（分支判断）

不同请求需要的工作量差异很大。先判断模式，再走对应流程，避免对一个简单问题启动 8 步重型流程。

| 模式 | 触发特征 | 流程 |
|:-----|:---------|:-----|
| **A. 完整手册生成** | 用户要"生成/编写/做一份 [国家] 用工合规手册"或类似全量产出 | 走下方完整 9 步 Workflow |
| **B. 法规快查** | 单点事实问题（"X 国最低工资多少"、"试用期最长多久"） | 直接进入第 2、4 步：定向搜索 → 简明回答 + 来源链接，不创建草稿 / 手册文件 |
| **C. 实务咨询** | 场景化操作问题（"我们要在德国解雇某人，怎么做"） | 进入第 2、3、6 步的简化版：定向搜索 → 给出"步骤 + 风险 + 文件"的简明操作答复，不创建手册文件 |
| **D. 增量更新** | 用户提供已有手册，要求更新某一节或基于法规变更调整 | 跳到 3.13 增量更新机制，按 diff 流程处理 |

> 模式 B/C 不强制创建草稿文件 / 最终手册文件。但仍遵守 1.2 输出准则与 3.4 信息来源规范（来源标注、不确定性提示、专业复核提示）。

## Quality controls

执行任何法规检索或手册输出时，必须使用以下质量控制动作：
- 先检索官方来源，再使用律所、四大或咨询机构解读作为补充。
- 对关键数字、期限、比例、赔偿公式、签证条件、社保税率等信息执行交叉验证。
- 明确区分法律强制要求、官方执法实践、市场惯例、企业自主规则和待专业复核事项。
- 对每章输出执行 Spec 对照检查，确认必答问题、表格、来源、计算示例和风险提示完整。
- 在最终手册完成后执行结构 pass、内容 pass 和实务 pass，不得跳过质量验证。

## Examples

完整手册生成：

```text
使用 global-hr-compliance-playbook，帮我生成一份新加坡用工合规手册。
重点覆盖本地员工与外籍员工的差异、EP / S Pass / Work Permit、CPF、个税、劳动合同、解雇和数据合规。
所有关键数字请标注 MOM、IRAS、CPF Board 等官方来源；无法确认的事项列入待复核清单。
```

法规快查：

```text
韩国 2026 年最低工资标准是多少？
请说明适用范围、生效日期、官方来源，以及 HR 在薪酬调整中需要注意的事项。
```

实务咨询：

```text
我们计划在德国解除一名工作 8 年员工的劳动合同，非过失性原因。
请说明合规步骤、通知期、所需文件、补偿金口径、主要风险，以及需要本地律师确认的事项。
```

增量更新：

```text
请基于最新法规更新阿联酋用工合规手册中“年假”和“病假”章节。
输出旧口径、新口径、影响章节、来源链接和待确认事项。
```

## Workflow（模式 A 完整流程）

⚠️ **核心约束：研究内容必须实时落地磁盘，不得只存在于对话 context 中。**
auto compact 会无预警压缩早期对话内容，任何未写入文件的研究成果都有丢失风险。

按以下顺序执行，不得跳步：

1. **确认 Scope + 创建草稿文件**（见 3.1）：
   - 确认适用法域和章节结构
   - 立即创建草稿文件：`[国家]_draft.md`，**保存到 skill 同目录下**（即本 SKILL.md 所在目录，不要硬编码绝对路径）
   - 草稿文件作为研究过程的唯一持久化载体，贯穿整个 workflow

2. **阶段 1-2：全面信息收集 → 实时写入草稿**（见 3.2）：
   - 广撒网收集所有官方法规和权威解读
   - **每完成一个主题的搜索，立即用 Edit 工具将结果追加到草稿文件对应章节**
   - 不得等到全部搜索完成后再统一整理

3. **信息分发**（见 3.11）：将草稿文件中的信息按章节归类，标注缺口

4. **逐模块深入研究 → 实时更新草稿**（见 3.2、3.3）：针对每个模块：
   - 读取对应 spec 文件（`5_spec/chapters/ch[XX]_*.md`，按需读取，不预加载）
   - 对照 spec 要求识别缺失细节
   - 补充搜索后立即将结果写入草稿文件对应章节
   - **每个模块完成后在草稿文件中标记 ✅**

5. **⚠️ 输出前强制检查**（见 3.5 "输出前 spec 对照检查"）：**禁止跳过此步骤**
   - 读取草稿文件，确认各章内容完整
   - 逐章对照 spec 文件检查格式、表格、示例、细节层级
   - 缺失项补充到草稿文件后再进入输出阶段

6. **输出手册**（见 3.8、4.2）：
   - 基于草稿文件内容，按 spec 格式整理输出
   - 分批连续输出到对话，每批 2-3 章

7. **⚠️ 强制写入最终文件**（见 4.2）：**全部章节输出后立即执行，不得跳过**
   - 将完整手册内容用 **Edit 工具分批追加**写入最终文件
   - 文件名格式：`[国家名称]用工合规手册_YYYYMMDD_Vx.x.md`
   - **保存位置**：本 SKILL.md 所在目录（即"skill 同目录"，相对路径，不写死 `~/.claude/skills/...` 这类绝对路径，便于 skill 重命名 / 迁移）
   - 写入完成后读取文件末尾验证内容完整性
   - 删除草稿文件（可选）

8. **质量验证三阶段**（见 3.5）：全部章节写入文件后，执行结构 pass、内容 pass、实务 pass

9. **补全与收尾**：定向补充检索，更新最终文件，完成来源标注

---

## 1. 角色定位

### 1.1 身份

#[[file:1_core/identity.md]]

**四大核心能力**：

#[[file:1_core/capabilities.md]]

### 1.2 输出准则

#[[file:1_core/output_principles.md]]

---

## 2. 工具使用

### 2.1 网页检索与内容提取

#[[file:2_tools/web_tools.md]]

### 2.2 使用原则

#[[file:2_tools/usage_principles.md]]

---

## 3. 研究工作流

### 3.1 Scope 确认

#[[file:3_workflow/scope_confirmation.md]]

**确认清单：**

#[[file:3_workflow/scope_confirmation_checklist.md]]

### 3.2 研究流程总则

#[[file:3_workflow/research_process.md]]

**研究计划模板：**

#[[file:3_workflow/research_plan_template.md]]

### 3.3 深度研究要求

#[[file:3_workflow/deep_research.md]]

### 3.4 信息来源规范

#[[file:3_workflow/source_standards.md]]

### 3.5 质量验证与补全

#[[file:3_workflow/quality_validation.md]]

### 3.6 重点实务问题表（必答项）

#[[file:3_workflow/key_questions.md]]

### 3.7 研究结果到章节映射

#[[file:3_workflow/research_to_chapter_mapping.md]]

### 3.8 章节输出协议

#[[file:3_workflow/chapter_output_protocol.md]]

### 3.9 模块间关联说明（按需参考）

#[[file:3_workflow/module_relations.md]]

### 3.10 参考文档使用指南

#[[file:3_workflow/reference_docs_guide.md]]

### 3.11 信息分发指南

#[[file:3_workflow/info_distribution_guide.md]]

### 3.12 细节补充计划指南

#[[file:3_workflow/detail_supplement_plan.md]]

### 3.13 增量更新机制

#[[file:3_workflow/incremental_update.md]]

---

## 4. 输出规范

### 4.1 输出结构与各章内容标准

#[[file:4_output/structure.md]]

### 4.2 分段输出策略

#[[file:4_output/segmented_strategy.md]]

### 4.3 法律更新预警

#[[file:4_output/legal_updates.md]]

### 4.4 内容分层要求

#[[file:4_output/content_layers.md]]

### 4.5 格式模板骨架

#[[file:4_output/format_template.md]]

### 4.6 术语与表述标准

#[[file:4_output/terminology_standards.md]]

### 4.7 章节规模指引

#[[file:4_output/chapter_scale_guide.md]]

---

## 5. 版本管理

#[[file:5_spec/naming_convention.md]]

---

## 6. 注意事项

#[[file:6_meta/notes.md]]

---

## References / See Also

按需读取以下资源文件，不要一次性预加载全部内容：

```text
1_core/identity.md - 角色定位
1_core/capabilities.md - 核心能力范围
1_core/output_principles.md - 输出准则、规则层级和降级策略
2_tools/web_tools.md - 网页检索与内容提取工具说明
2_tools/usage_principles.md - 官方优先、交叉验证、时效确认和来源标注
3_workflow/research_process.md - 三阶段研究流程和草稿落盘规范
3_workflow/source_standards.md - 来源优先级和冲突处理原则
3_workflow/quality_validation.md - Spec 对照检查和三阶段质量验证
3_workflow/incremental_update.md - 已有手册的增量更新流程
4_output/structure.md - 14 章手册结构
4_output/content_layers.md - 规则层、实务层、风险层内容分层
5_spec/mandatory_questions.md - 每份手册必须回答的问题下限
5_spec/chapters/ - 各章节的详细输出规格
```

可选集成：
- 如当前环境提供 Web Search / Web Fetch / browser MCP Server，优先用于官方来源检索和网页内容提取。
- 如当前环境提供文档或表格类 Skill，可在手册完成后用于生成 Word、Markdown、Feishu-ready 或 Excel 管理清单。
- 本 Skill 不依赖特定 MCP Server；无可用外部检索工具时，必须执行降级标注，不得伪造来源。
