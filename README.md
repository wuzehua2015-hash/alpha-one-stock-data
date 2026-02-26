# AlphaOne Stock Data

AlphaOne股票分析系统的历史数据仓库

## 数据范围

- **股票数量**: 1000只（主板、中小板、创业板抽样）
- **时间范围**: 2023-01-01 至 2025-12-31
- **数据类型**: 日线数据（开高低收成交量）

## 目录结构

```
.
├── README.md
├── stocks.json              # 股票列表（代码、名称、行业、市值）
├── daily/                   # 个股日线数据
│   ├── 000001.csv
│   ├── 000002.csv
│   └── ...
├── index/                   # 指数数据
│   ├── sh000001.csv         # 上证指数
│   ├── sz399001.csv         # 深证成指
│   └── sz399006.csv         # 创业板指
└── update_log.txt           # 更新日志
```

## 数据格式

### 个股数据 (daily/XXXXXX.csv)

```csv
date,open,high,low,close,volume,amount
2023-01-03,15.20,15.50,15.10,15.35,1234567,18923456
...
```

### 股票列表 (stocks.json)

```json
[
  {
    "symbol": "000001",
    "name": "平安银行",
    "sector": "银行",
    "market_cap": 250000000000,
    "board": "主板"
  }
]
```

## 抽样策略

| 板块 | 数量 | 选择标准 |
|------|------|---------|
| 主板大盘 | 300只 | 市值>500亿 |
| 主板中盘 | 300只 | 市值100-500亿 |
| 主板小盘 | 200只 | 市值<100亿 |
| 中小板 | 150只 | 002开头 |
| 创业板 | 50只 | 300开头 |

## 使用说明

### 回测使用
```python
import pandas as pd

# 读取个股数据
df = pd.read_csv('daily/000001.csv')
df['date'] = pd.to_datetime(df['date'])
df = df.set_index('date')

# 读取股票列表
import json
with open('stocks.json', 'r') as f:
    stocks = json.load(f)
```

### 数据更新
数据每日收盘后自动更新，提交记录见 `update_log.txt`

## 免责声明

本数据仅供研究学习使用，不构成投资建议。数据来源于公开API，可能存在延迟或错误，请以交易所官方数据为准。

## License

MIT License
