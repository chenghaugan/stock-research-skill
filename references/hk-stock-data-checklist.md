# 港股数据采集清单

> 所有端点来自 `global-stock-data` skill 及 Tavily MCP。采集结果保存到 `data/` 子目录。

## 采集前置

**调用 `global-stock-data` Skill 完成以下所有端点**。每条数据标注采集时间戳和来源。

---

## 一、行情与财务（Lane 1）

### 1.1 实时行情
- **行情快照**：新浪财经港股（25字段）/ 腾讯财经港股（78字段）
- **东财 push2**：secid 实时行情（中文名/涨跌幅/换手率）
- **K线数据**：Yahoo chart 港股日K（最近250个交易日）

### 1.2 财务数据
- **三表**：东财 datacenter 港股三表（资产负债表/利润表/现金流量表）
- **关键指标**：东财 datacenter GMAININDICATOR（PE/PB/ROE/利润率）
- **Yahoo crumb**：23个模块（财务数据+关键指标+分析师+机构持仓）

### 1.3 一致预期
- Yahoo crumb 分析师预测（EPS预测/评级/目标价区间）

### 1.4 资金面
- **资金流**：东财 push2his 日级资金流（主力/大单/中单/小单）

### 时效要求

| 数据 | 时效标准 |
|------|:------:|
| 股价/市值/PE/PB | ≤1 天 |
| 财务数据 | 最新报告期，距截止日 ≤6 个月 |
| 一致预期 | 预测期覆盖当前财年或下一财年 |

---

## 二、研报与预期（Lane 2）

**港股研报主要通过搜索获取**：

Tavily 搜索：
- `"[公司名称] 港股 券商研报 目标价 最近3个月"`
- `"[公司名称] 业绩交流会 纪要 FY2025"`
- `"[公司名称] [股票代码] analyst report target price 2026"`
- `"[公司名称] 管理层 战略规划 2026"`

时效：优先 ≤6 个月内的研报。

---

## 三、新闻与公告（Lane 3）

### 3.1 新闻
- Yahoo search 港股新闻（按代码）

### 3.2 Tavily + web-access 补充
- `"[公司名称] 最新新闻 最近7天"`
- `"[公司名称] 重大公告 2026"`
- `"[公司名称] 香港联交所 公告"`

### 时效要求
≤7 天的 3x 权重，≤30 天的 2x 权重。

---

## 四、行业与竞争（Lane 4）

### 4.1 Tavily 搜索
- `"[行业名称] 市场规模 2025 2026 增速 中国"`
- `"[行业名称] 港股 竞争格局 市场份额"`
- `"[公司名称] 港股 竞争对手 对比"`
- `"[company name] Hong Kong stock industry analysis 2026"`

### 4.2 竞对公司采集
确定 3-5 家可比公司后，对每家执行：
- 新浪/腾讯行情快照
- 东财 datacenter GMAININDICATOR
- Yahoo crumb 分析师预期（如有）

保存到 `data/competitors_<timestamp>.md`。

---

## 五、数据保存格式

```
data/
├── market_finance_<YYYYMMDD>.md
├── research_reports_<YYYYMMDD>.md
├── news_announcements_<YYYYMMDD>.md
├── industry_competition_<YYYYMMDD>.md
└── competitors_<YYYYMMDD>.md
```

每个文件头部标注采集时间戳、数据来源、标的。
