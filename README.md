# Edgen 图表与组件

## 组件分类总览

```
组件
├── 通用图表组件
│   └── AI 生成全部数据，前端纯渲染（BarChart, LineChart, PieChart 等 21 种）
│
└── 业务组件
    ├── 复杂组件
    │   └── 复用产品已有组件或新设计带复杂交互的卡片（PriceHistoryChart, SectorHeatmap 等）
    │
    └── 简单引导组件
        └── 单一行为：点击跳转，不同跳转类型可有不同样式
```

## 文档导航

| 分类 | 文档 | 说明 |
|---|---|---|
| 通用图表 | [chart-design-guide.md](./chart-design-guide.md) | 通用图表需求指南：如何提出新图表需求 |
| 通用图表 | [chart-examples.md](./chart-examples.md) | 通用图表样式说明与使用场景参考 |
| 业务组件 | [business-component-guide.md](./business-component-guide.md) | 业务组件需求指南：如何提出业务组件需求 |

## 如何选择？

1. **数据由 AI 生成**（对比分析、趋势总结、排名等）→ **通用图表**，参考 chart-design-guide.md
2. **需要带复杂交互的组件**（复用产品已有组件或新设计，前端调 API 取实时数据）→ **业务组件 · 复杂组件**，参考 business-component-guide.md
3. **只需引导用户跳转**（单一点击行为，跳转到详情页、功能页等）→ **业务组件 · 简单引导组件**，参考 business-component-guide.md
