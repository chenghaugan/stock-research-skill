# 美股数据采集清单

> 所有端点来自 `global-stock-data` skill 及 Tavily MCP。采集结果保存到 `data/` 子目录。

## 采集前置

**调用 `global-stock-data` Skill 完成以下所有端点**。每条数据标注采集时间戳和来源。

---

## 一、行情与财务（Lane 1）

### 1.1 实时行情
- **行情快照**：新浪财经美股（36字段）/ 腾讯财经美股（71字段）
- **东财 push2**：secid 实时行情（中文名/涨跌幅/换手率）
- **K线数据**：Yahoo chart 美股日K（最近250个交易日）

### 1.2 财务数据
- **三表**：东财 datacenter 美股三表（资产负债表/利润表/现金流量表）
- **关键指标**：东财 datacenter GMAININDICATOR
- **Yahoo crumb**：23个模块（财务数据+关键指标+分析师+机构持仓）
- **SEC EDGAR XBRL**：503个GAAP指标（营收/净利/EPS等，最近5年）

### 1.3 一致预期
- Yahoo crumb 分析师预测（EPS预测/评级/目标价区间/目标价均值）

### 1.4 机构持仓
- Yahoo crumb 机构持仓（前十大机构/持股比例）

### 1.5 资金面
- **资金流**：东财 push2his 日级资金流（主力/大单/中单/小单）

### 1.6 SEC Filing
- EDGAR submissions（10-K/10-Q/8-K，最近4次Filing）

### 时效要求

| 数据 | 时效标准 |
|------|:------:|
| 股价/市值/PE/PB | ≤1 天 |
| 财务数据 | 最新报告期，距截止日 ≤6 个月 |
| 一致预期 | 预测期覆盖当前财年或下一财年 |
| SEC Filing | 最近一次 10-K/10-Q |

---

## 二、研报与预期（Lane 2）

**美股研报主要通过搜索获取**：

Tavily 搜索：
- `"[company name] [ticker] analyst report target price 2026"`
- `"[ticker] earnings call transcript latest quarter"`
- `"[company name] stock rating consensus price target"`
- `"[ticker] Wall Street analyst upgrade downgrade recent"`

web-access 补充：访问 Seeking Alpha / Yahoo Finance 等获取最新观点。

时效：优先 ≤6 个月内的研报和评级调整。

---

## 三、新闻与公告（Lane 3）

### 3.1 新闻
- Yahoo search 美股新闻（按代码）

### 3.2 SEC Filing（公告等效）
- EDGAR 8-K（重大事项公告）
- EDGAR 10-Q/10-K（季报/年报）

### 3.3 Tavily + web-access 补充
- `"[company name] news last 7 days"`
- `"[ticker] breaking news 2026"`
- `"[company name] management guidance 2026"`

### 时效要求
≤7 天的 3x 权重，≤30 天的 2x 权重。SEC Filing 按提交日期排序。

---

## 四、行业与竞争（Lane 4）

### 4.1 Tavily 搜索
- `"[industry] market size growth rate 2025 2026"`
- `"[industry] competitive landscape market share 2026"`
- `"[company name] [ticker] competitors comparison analysis"`
- `"[company name] SWOT analysis"`

### 4.2 竞对公司采集
确定 3-5 家可比公司后，对每家执行：
- 新浪/腾讯行情快照
- 东财 datacenter GMAININDICATOR
- Yahoo crumb 分析师预期+机构持仓

保存到 `data/competitors_<timestamp>.md`。

---

## 五、数据保存格式

```
data/
├── market_finance_<YYYYMMDD>.md
├── research_reports_<YYYYMMDD>.md
├── news_filings_<YYYYMMDD>.md
├── industry_competition_<YYYYMMDD>.md
└── competitors_<YYYYMMDD>.md
```

每个文件头部标注采集时间戳、数据来源、标的。
