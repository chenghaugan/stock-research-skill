# Data Collection Manifest

> 标的：{TICKER} — {COMPANY_NAME}
> 市场：{MARKET}
> 开始时间：{START_TIME}

## Collection Progress

### Lane 1: 行情与财务
- [ ] 实时行情（股价/涨跌幅/成交量/市值）
- [ ] PE/PB/估值指标
- [ ] K线数据（最近250个交易日）
- [ ] 财务报表（利润表/资产负债表/现金流量表）
- [ ] 关键财务指标（ROE/ROA/毛利率/净利率）
- [ ] 一致预期EPS（当前财年+下一财年）
- [ ] 资金流向（主力/大单/中单/小单）

### Lane 2: 研报与预期
- [ ] 最新研报列表（最近30条）
- [ ] 评级分布统计
- [ ] 目标价共识
- [ ] 管理层业绩会纪要
- [ ] 同行评级对比

### Lane 3: 新闻与公告
- [ ] 个股新闻（最近20条）
- [ ] 重大公告
- [ ] 行业政策动态

### Lane 4: 行业与竞争
- [ ] 行业规模与增速
- [ ] 竞争格局分析
- [ ] 竞对公司列表（3-5家）
- [ ] 竞对公司财务+估值数据

## Data Freshness Validation

| 检查项 | 标准 | 实际值 | ✓/✗ |
|--------|------|--------|-----|
| 股价日期 | ≤1天 | | |
| 财报截止日 | ≤6个月 | | |
| 一致预期年份 | 当前/下一财年 | | |
| 研报日期 | ≤6个月 | | |
| 最新新闻 | ≤7天 | | |

## Files Saved

| 文件名 | 行数 | 状态 |
|--------|:---:|:---:|
| data/market_finance_<DATE>.md | | |
| data/research_reports_<DATE>.md | | |
| data/news_announcements_<DATE>.md | | |
| data/industry_competition_<DATE>.md | | |
| data/competitors_<DATE>.md | | |
