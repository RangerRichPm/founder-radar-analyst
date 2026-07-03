# 风险框架定义（5 节点 / 18 风险 / 7 维度）

## 节点 → 触发事件映射

| 节点 ID | 中文名 | 触发事件 | 风险数 |
| --- | --- | --- | --- |
| `financing` | 融资节点 | `capital:angel_round` / `a_round` / `b_round` / `c_round` | 5 |
| `overseas` | 出海节点 | `overseas:office_setup` / `hiring` / `market_entry` / `data_transfer` | 5 |
| `contract` | 合同节点 | `contract:big_client` / `gov_project` / `bidding` | 3 |
| `expansion` | 扩张节点 | `expansion:mass_hiring` / `new_city` / `new_business` | 4 |
| `renewal` | 续保节点 | `other:insurance_expiry` | 2 |

## 节点 → 风险 详细映射

### financing (融资节点)
- `founder_key_person` 关键人风险 [founder / high]
- `founder_health` 创始人健康风险 [founder / medium]
- `do_liability` 董监高责任风险 [capital / high]
- `equity_risk` 股权结构风险 [capital / medium]
- `expansion_risk` 扩张风险 [capital / medium]

### overseas (出海节点)
- `cross_border_liability` 跨境责任风险 [capital / high]
- `overseas_employment` 海外雇佣风险 [employee / high]
- `data_compliance` 数据合规风险 [business / high]
- `product_liability_overseas` 海外产品责任 [business / medium]
- `cybersecurity_overseas` 海外网络安全 [business / medium]

### contract (合同节点)
- `contract_liability` 合同责任风险 [business / high]
- `performance_risk` 履约风险 [business / medium]
- `public_liability` 公众责任风险 [business / medium]

### expansion (扩张节点)
- `employer_liability` 雇主责任风险 [employee / high]
- `labor_dispute` 劳动争议风险 [employee / high]
- `core_employee_loss` 核心员工流失 [employee / medium]
- `office_relocation` 办公室搬迁风险 [business / low]

### renewal (续保节点)
- `coverage_gap` 保障缺口风险 [capital / high]
- `policy_lapse` 保单失效风险 [capital / medium]

## 风险分层 (Risk Layers)

- `founder` 创始人层（个人风险）
- `capital` 资本层（融资/股权/治理）
- `employee` 员工层（雇佣/团队）
- `business` 业务层（合同/产品/数据/履约）

## 严重度 (Severity)

- `high` 高（必须预警）
- `medium` 中（建议关注）
- `low` 低（仅供参考）

## 7 维度诊断框架（risk-chat 用）

风险诊断对话采用 7 维度评估，每维度评分 0-10：

1. **创始人** — 健康 / 关键人 / 决策力
2. **团队** — 核心人员稳定 / 组织能力
3. **客户** — 集中度 / 流失率 / 大单依赖
4. **产品** — 技术成熟度 / 差异化 / 迭代节奏
5. **数据** — 合规 / 跨境 / 备份
6. **出海** — 当地法规 / 雇佣 / 税务
7. **治理** — 股权 / 董事会 / 关联交易

## 风险推理源标签 (reasoningSource)

每条风险必须标注来源：

- `rule_hits` 规则引擎命中（确定性高）
- `llm_added` LLM 推理新增（5 类隐性风险）
- `llm_calibrated` LLM 校准（行业严重度调整）
- `case_reference` 行业案例佐证（从 industry-cases.json 检索）

## LLM 推理：5 类隐性风险

`llm_added` 触发条件（任意一条满足）：

1. **决策疲劳**：连续 6+ 个月高密度融资/扩张
2. **信任透支**：高管离职 + 创始人频繁发声
3. **合规盲区**：出海+数据 OR 金融+无牌照
4. **现金流压力**：融资消息少 + 团队翻倍
5. **组织失序**：新城市 + 新业务 + 新融资同时发生

## LLM 校准：7 行业严重度调整

`llm_calibrated` 行业敏感度倍率：

- 金融 / 医疗 / 教育 / 跨境：1.5x
- AI / SaaS：1.2x
- 其他：1.0x
