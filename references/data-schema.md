# 数据 Schema 定义

## 1. `data/industry-cases.json`

行业案例库核心数据结构。当前 v3.0 含 62 案例 / 24 行业 / 5 节点 / 10 区域。

```json
{
    "version": "3.0",
    "updatedAt": "2026-07-03",
    "description": "行业风险案例库 v3.0 — 按【行业 × 节点 × 风险 × 区域】四维索引。",
    "cases": [
        {
            "id": "case_001",
            "title": "某SaaS公司A轮后创始人突发疾病",
            "industry": ["b2b_saas", "ai_saas"],
            "nodes": ["financing"],
            "riskType": "founder_key_person",
            "riskName": "关键人风险",
            "year": 2022,
            "region": "CN",
            "verified": true,
            "url": "https://example.com/source",
            "description": "详细事件描述（200-500 字）",
            "loss": "具体损失量化（如：估值缩水 60%）",
            "lossAmount": 6000000,
            "lesson": "教训与启示（50-150 字）",
            "source": "公开报道综合 / 监管处罚公告 / 法律判例"
        }
    ],
    "industries": { "ai": "AI / 大模型", ... },
    "nodes": { "financing": "融资节点", ... },
    "regions": { "CN": "中国", "US": "美国", "EU": "欧盟", ... }
}
```

### M3.2 新增字段

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `region` | string | 推荐 | 区域代码：CN/US/EU/SEA/JP/KR/IN/ME/LATAM/Global |
| `verified` | boolean | 推荐 | 是否为真实可验证案例 |
| `url` | string | 可选 | 来源 URL，用于追溯 |
| `lossAmount` | number | 可选 | 量化损失金额（统一为人民币元） |

### 检索 API（case-library.js V3）

```javascript
// 加载（首次调用触发 fetch，含 schema 校验）
const data = await window.CaseLibrary.load();

// 四维检索（M3.2 新增 region）
const matches = window.CaseLibrary.search({
    industry: 'ai',
    node: 'accident',
    riskType: 'ip_lawsuit',
    region: 'US',
    limit: 5
});

// 按风险类型推荐案例
const matched = window.CaseLibrary.recommendForRisks(risks, 'ai', 3);

// M3.2: 运行时添加新案例（自动校验 + 去重）
const result = window.CaseLibrary.addCase(newCaseObj);

// M3.2: 批量添加
const { added, skipped, errors } = window.CaseLibrary.addCases(caseArray);

// M3.2: 校验单条案例
const { valid, errors } = window.CaseLibrary.validateCase(caseObj);

// M3.2: 统计信息
const stats = window.CaseLibrary.stats();
// { total: 62, byRegion: {US:22, CN:19, ...}, byNode: {...}, byIndustry: {...}, verified: 62, customAdded: 0 }

// M3.2: 按区域查询
const usCases = window.CaseLibrary.byRegion('US', 10);
```

### 区域代码表

| 代码 | 区域 |
| --- | --- |
| `CN` | 中国 |
| `US` | 美国 |
| `EU` | 欧盟 |
| `SEA` | 东南亚 |
| `JP` | 日本 |
| `KR` | 韩国 |
| `IN` | 印度 |
| `ME` | 中东 |
| `LATAM` | 拉美 |
| `Global` | 全球/多区域 |

## 2. 报告数据结构 (Report)

`window.app.currentReport` 与 `sessionStorage['founderRadarReport']` 共享此结构：

```javascript
{
    company: {
        name: "企业名",
        industry: "industry_code",
        industryName: "行业中文名",
        foundedYear: 2021,
        teamSize: "11-50",
        headquarters: "北京",
        fundingStage: "pre-a",
        coreBiz: "核心业务描述"
    },
    founder: {
        name: "创始人姓名",
        role: "CEO & 联合创始人"
    },
    nodes: [
        { id: 'financing', name: '融资节点', events: ['capital:a_round'] }
    ],
    risks: [
        {
            id: "founder_key_person",
            name: "关键人风险",
            layer: "founder",
            severity: "high",
            sourceNodes: ['financing'],
            reasoningSource: "rule_hits",  // rule_hits | llm_added | llm_calibrated
            description: "风险描述",
            watchPoints: ["关注点 1", "关注点 2"]
        }
    ],
    cases: [
        { id: 'case_001', title: '...', relevance: 0.85 }
    ],
    insights: [
        { text: "综合洞察 1", type: "cross_node" }
    ],
    generatedAt: "2026-07-02T12:00:00Z",
    disclaimer: "本报告基于 2026-07-02 公开信息推测..."
}
```

## 3. 触达话术数据结构 (outreach.js)

```javascript
{
    company: { name, industry, stage, ... },
    scripts: [
        {
            channel: "A",  // A=共同朋友 / B=微信 / C=LinkedIn / D=邮件
            channelName: "共同朋友/校友/投资人介绍",
            priority: 1,  // 1=首选, 2-4=次选
            subject: "邮件主题 / 微信开场白",
            body: "正文（200-500 字）",
            forbiddenHits: [],  // 自检命中的违禁词
            template: "alumni_intro"  // 模板 ID
        }
    ],
    primaryChannel: "A",
    compliancePassed: true
}
```

### 触达话术模板

| 模板 ID | 渠道 | 适用场景 |
| --- | --- | --- |
| `alumni_intro` | A | 共同朋友/校友 |
| `investor_intro` | A | 投资人介绍 |
| `wechat_cold` | B | 微信冷启动 |
| `wechat_follow` | B | 微信复访 |
| `linkedin_cold` | C | LinkedIn 冷启动 |
| `email_formal` | D | 邮件正式 |
| `email_followup` | D | 邮件跟进 |

## 4. 风险对话数据结构 (risk-chat.js)

```javascript
{
    riskMap: {
        founder:  { score: 0-10, evidence: ["..."], notes: "..." },
        team:     { score: 0-10, evidence: ["..."], notes: "..." },
        customer: { score: 0-10, evidence: ["..."], notes: "..." },
        product:  { score: 0-10, evidence: ["..."], notes: "..." },
        data:     { score: 0-10, evidence: ["..."], notes: "..." },
        overseas: { score: 0-10, evidence: ["..."], notes: "..." },
        governance: { score: 0-10, evidence: ["..."], notes: "..." }
    },
    messages: [
        { role: "user", text: "..." },
        { role: "assistant", text: "..." }
    ],
    updatedAt: "ISO date string"
}
```

## 5. 行业案例库扩展（WebSearch 增量）

M3.2 起支持从 WebSearch 检索新事件后，按以下流程入库：

```javascript
function normalizeCase(article) {
    return {
        id: `case_${Date.now()}`,
        title: article.title,
        industry: mapIndustryFromContent(article.content),  // LLM 分类
        nodes: extractNodes(article.content),  // LLM 抽取触发节点
        riskType: extractRiskType(article.content),  // LLM 抽取风险类型
        riskName: getRiskName(riskType),
        year: new Date(article.publishedAt).getFullYear(),
        region: extractRegion(article.content),  // M3.2: LLM 抽取区域
        verified: true,                           // M3.2: 标记为已验证
        url: article.url,                         // M3.2: 来源 URL
        description: article.summary,
        loss: extractLoss(article.content),       // LLM 抽取损失
        lossAmount: extractLossAmount(article.content),  // M3.2: 量化损失
        lesson: extractLesson(article.content),   // LLM 抽取教训
        source: article.url
    };
}

// M3.2: 使用 CaseLibrary.addCase() 入库（自动校验 + 去重）
const result = window.CaseLibrary.addCase(normalizeCase(article));
if (!result.success) {
    console.warn('案例入库失败:', result.error);
}
```

### 增量更新检查项

入库前必须人工或 LLM 二次确认：
- [ ] 来源 URL 公开可访问
- [ ] 不是付费墙后内容
- [ ] 不是匿名爆料（需有可核实媒体来源）
- [ ] 不涉及未公开诉讼细节
- [ ] 描述中不含个人隐私信息
