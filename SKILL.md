---
name: founder-radar-analyst
description: |
  This skill should be used when the user wants to perform 7-stage risk diagnostics
  on early-stage companies for lead generation/insurance-free advisory work, including:
  node discovery (financing/overseas/expansion/contract/accident signals),
  company/founder intel aggregation, rule-based + LLM-enhanced risk reasoning,
  independent shareable radar card generation, multi-channel outreach scripts,
  and 7-dimension risk diagnostic chat.
  Triggers: "Founder Radar", "创始人风险", "节点发现", "风险预警卡",
  "触达话术", "风险诊断", "node discovery", "radar card",
  "earlier-stage company risk", "lead generation without selling insurance".
agent_created: true
---

# Founder Radar Analyst V1

> AI 驱动的创业公司风险情报系统 · 七阶段飞轮 · 7-Stage Flywheel

## 核心原则（不可妥协）

**不卖保险**（先卖洞察）· **不找企业**（先找节点）· **不推产品**（先做诊断）

合规细节见 `references/compliance.md`。**加载 Skill 前必须先读此文件。**

## Skill 目录结构

```
founder-radar-analyst/
├── SKILL.md                   # 本文件（入口）
└── references/                # 方法论与框架文档
    ├── methodology.md         # 7 阶段飞轮详解 + 触发词库
    ├── risk-framework.md      # 5节点/18风险/7维度 定义
    ├── compliance.md          # 合规与伦理边界（必读）
    ├── data-schema.md         # 数据结构定义
    └── case-library.md        # 62条行业案例库（四维索引）
```

## 七阶段飞轮

```
节点发现 → 企业分析 → 创始人画像 → 风险推理 → 预警卡 → 触达话术 → 诊断对话
  ①          ②          ③          ④        ⑤        ⑥         ⑦
```

详细方法论见 `references/methodology.md`。

## 使用流程

### Step 0: 加载必要 References

加载 Skill 后，**必须依次读取**：

1. `references/compliance.md` — 3 条不可越界的红线
2. `references/methodology.md` — 7 阶段定义
3. `references/risk-framework.md` — 风险/节点 schema
4. `references/data-schema.md` — 报告/案例/话术数据结构
5. `references/case-library.md` — 62 条行业案例库

### Step 1: 标准调用路径

按用户意图选择起点，AI 自行按方法论推理执行（不依赖任何 JS 代码）：

| 用户输入 | 起点阶段 | 工作流 |
| --- | --- | --- |
| "帮我做一份早期企业风险诊断" | ① 节点发现 | 跑全 7 阶段 |
| "给一家刚融完 A 轮的 SaaS 评估" | ② 手动录入 | 跳过 ①，情报聚合 → ④ ⑤ ⑥ ⑦ |
| "生成一份风险预警卡" | ⑤ | 用现有情报 → 推理 → 输出预警卡 |
| "出触达话术" | ⑥ | 选渠道 → 生成 → 合规自检 |
| "7 维度风险对话" | ⑦ | 选维度 → 自由提问 |
| "node discovery" | ① | 5 类信号扫描 |

### Step 2: 数据采集

- **节点发现**：用 WebSearch 检索赛道 + 融资/出海/扩张/合同/事故信号
- **企业情报**：用 WebSearch + WebFetch 官网/百科/招聘网站补全 9 维度画像
- **创始人画像**：仅采集 4 维度公开信息（背景/重心/表达/挑战），不碰隐私
- **案例匹配**：从 `references/case-library.md` 按行业×节点×风险×区域四维匹配

### Step 3: 风险推理

**双层推理**：
1. **规则引擎**：根据 `risk-framework.md` 的 5 节点 × 18 风险映射表自动命中
2. **LLM 增强**：识别 5 类隐性风险（决策疲劳/信任透支/合规盲区/现金流压力/组织失序），按行业严重度校准（金融/医疗/教育/跨境 1.5x，AI/SaaS 1.2x）

### Step 4: 输出

根据起点阶段生成对应产物：
- **预警卡**：独立 HTML 页，包含风险热区、行业案例匹配、AI 推理文本、时效标识、免责声明
- **触达话术**：4 渠道（A 介绍/B 微信/C LinkedIn/D 邮件）× 7 模板，含 15 词违禁词自检
- **诊断报告**：7 维度评分 + 风险地图 + 建议行动

## 重要提示

### 合规自检
每次生成触达话术后，**强制自检 15 词违禁词**：
- 核心 12 词：保额/报价/投保/险种/产品链接/销售/加微/私聊/优惠/限时/保单/退保
- 补充 3 词：保险/保费/理赔
- 命中任一需重新生成或人工干预。

### 创始人画像边界
绝不做性格打分 / 心理推测 / 家庭深挖 / 存储个人手机邮箱。
完整边界见 `references/compliance.md`。

### 案例库引用
在预警卡/话术中引用案例时，标注案例编号（如 case_r002）和来源。
用户新增案例时，按 `references/data-schema.md` 第 5 节的 normalizeCase 流程处理。

## 触发词库

完整触发词见 `references/methodology.md` 末节。核心触发词：

- Founder Radar / founder radar / 创始人雷达
- 风险预警卡 / radar card / 节点发现 / node discovery
- 触达话术 / outreach / 风险诊断 / risk chat
- 早期企业风险 / earlier-stage risk
- 7 阶段飞轮 / 7-stage flywheel
- 不卖保险 / insurance-free advisory

## 版本

- **V1 M1** 节点发现 / 企业情报 / 创始人画像 / 风险预警卡
- **V1 M2** LLM 推理增强 / 触达话术 / 风险诊断对话 / 行业案例库
- **V1 M3.1** dataAdapter 三模式 + IntelAggregator 9 维度自动补全
- **V1 M3.2** 真实全球案例库扩充（62 条 / 10 区域）+ CaseLibrary 工程化升级
- **V1 M4** Skill 纯化：移除网页应用代码，案例库转 Markdown，SKILL.md 精简为 AI 推理专用
