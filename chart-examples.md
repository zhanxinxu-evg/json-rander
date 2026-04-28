# Edgen 图表参考示例

> 所有图表类型的完整 JSON 示例、JSON Schema 与选型参考。
>
> 📖 设计新图表请参考主文档：[chart-design-guide.md](./chart-design-guide.md)

---

## 目录

- [图表类型总览](#图表类型总览)
- [各图表建议数据量](#各图表建议数据量)
- **基础统计类**：[BarChart](#barchart--柱状图) · [LineChart](#linechart--折线图) · [PieChart](#piechart--饼图) · [StatCallout](#statcallout--数据卡片) · [StatGrid](#statgrid--数据网格)
- **对比分析类**：[ComparisonBar](#comparisonbar--前后对比) · [VersusChart](#versuschart--对决对比) · [DumbbellChart](#dumbbellchart--哑铃图) · [LollipopChart](#lollipopchart--棒棒糖图)
- **排名与分布类**：[RankingBar](#rankingbar--排行榜) · [FunnelChart](#funnelchart--漏斗图) · [TreemapChart](#treemapchart--树图) · [BenchmarkHeatmap](#benchmarkheatmap--基准热力图)
- **流程与关系类**：[FlowDiagram](#flowdiagram--流程图) · [TimelineDiagram](#timelinediagram--时间线) · [SankeyChart](#sankeychart--桑基图) · [NetworkGraph](#networkgraph--网络关系图)
- **高级分析类**：[WaterfallChart](#waterfallchart--瀑布图) · [GaugeChart](#gaugechart--仪表盘) · [QuadrantMatrix](#quadrantmatrix--四象限矩阵) · [SensitivityChart](#sensitivitychart--敏感性分析)

---

## 图表类型总览

| 类别 | 图表类型 | 一句话描述 | 核心字段 |
|---|---|---|---|
| **基础统计** | BarChart | 不同类别的数值比较 | `values[{label, value}]` |
| | LineChart | 时间趋势变化 | `values[{label, value}]`, `comparison?` |
| | PieChart | 整体各部分占比 | `segments[{label, value}]` |
| | StatCallout | 突出单个关键指标 | `value`, `comparison?` |
| | StatGrid | 同时展示多个关键指标 | `stats[{label, value}]` |
| **对比分析** | ComparisonBar | 两个状态的前后对比 | `before{label,value}`, `after{label,value}` |
| | VersusChart | 两实体多维度对比 | `entityA`, `entityB`, `metrics[]` |
| | DumbbellChart | 多类别范围变化 | `dumbbellPoints[{label, start, end}]` |
| | LollipopChart | 突出特定项的对比 | `lollipopItems[{label, value, highlight?}]` |
| **排名与分布** | RankingBar | 有序排名 | `items[{label, value}]` |
| | FunnelChart | 转化漏斗 | `funnelStages[{label, value}]` |
| | TreemapChart | 层级占比 | `treemapItems[{label, value}]` |
| | BenchmarkHeatmap | 多实体多维度评分 | `dimensions[]`, `benchmarkEntities[]` |
| **流程与关系** | FlowDiagram | 线性流程步骤 | `stages[{label, stat?, sublabel?}]` |
| | TimelineDiagram | 事件时间线 | `events[{date, label, impact?}]` |
| | SankeyChart | 资金/流量流向 | `nodes[]`, `links[]` |
| | NetworkGraph | 实体关系网络 | `networkNodes[]`, `networkEdges[]` |
| **高级分析** | WaterfallChart | 累积增减变化 | `items[{label, value, type}]` |
| | GaugeChart | 指标在范围中的位置 | `value`, `max`, `context?` |
| | QuadrantMatrix | 二维空间定位 | `xAxis`, `yAxis`, `quadrantEntities[]` |
| | SensitivityChart | 变量变化影响分析 | `inputLabel`, `sensitivityEntities[]` |

---

## 各图表建议数据量

| 图表 | 最小 | 推荐 | 最大 |
|---|---|---|---|
| BarChart | 3 项 | 4-6 项 | 8 项 |
| LineChart | 4 点 | 6-8 点 | 12 点 |
| PieChart | 3 扇区 | 4-5 扇区 | 6 扇区 |
| RankingBar | 3 项 | 5-7 项 | 10 项 |
| VersusChart | 3 维度 | 4-5 维度 | 6 维度 |
| FunnelChart | 3 阶段 | 4-5 阶段 | 6 阶段 |
| NetworkGraph | 3 节点 | 4-6 节点 | 8 节点 |
| BenchmarkHeatmap | 3×3 | 3-4×4-5 | 5×6 |
| SankeyChart | 4 节点 | 5-6 节点 | 8 节点 |
| TreemapChart | 4 块 | 5-6 块 | 8 块 |

---

## 完整示例与 JSON Schema

> 每个图表包含：一句话描述、截图、JSON 示例数据、chartData 的 JSON Schema。

### 基础统计类

### BarChart — 柱状图

展示不同类别的数值比较。

![BarChart](chart-images/01-BarChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "BarChart",
      "props": {
        "chartData": {
          "type": "BarChart",
          "label": "2024年各季度营收",
          "values": [
            { "label": "Q1'24", "value": 12.5 },
            { "label": "Q2'24", "value": 14.8 },
            { "label": "Q3'24", "value": 16.2 },
            { "label": "Q4'24", "value": 18.1 }
          ],
          "unit": "$B"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "柱状图：用等宽柱子的高度对比不同类别的数值大小",
  "required": ["type", "label", "values"],
  "properties": {
    "type": { "type": "string", "const": "BarChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "数值单位，显示在数值旁" },
    "values": {
      "type": "array",
      "description": "每根柱子对应一个 label-value 对，label 显示在 X 轴，value 决定柱子高度",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "柱子的类别标签" },
          "value": { "type": "number", "description": "柱子的数值高度" }
        }
      },
      "minItems": 3,
      "maxItems": 8
    }
  }
}
```

</details>

---

### LineChart — 折线图

展示时间趋势变化。

![LineChart](chart-images/02-LineChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "LineChart",
      "props": {
        "chartData": {
          "type": "LineChart",
          "label": "月活跃用户趋势",
          "values": [
            { "label": "Jan", "value": 120 },
            { "label": "Feb", "value": 135 },
            { "label": "Mar", "value": 158 },
            { "label": "Apr", "value": 192 },
            { "label": "May", "value": 210 }
          ],
          "unit": "K",
          "comparison": "+75% in 5 months"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "折线图：用折线连接各时间点的数值，展示随时间的变化趋势",
  "required": ["type", "label", "values"],
  "properties": {
    "type": { "type": "string", "const": "LineChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "数值单位" },
    "comparison": { "type": "string", "description": "趋势总结文本，如 '+75% in 5 months'" },
    "values": {
      "type": "array",
      "description": "按时间顺序排列的数据点，相邻点之间用折线相连",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "时间点标签，如 'Jan'、'Q1'" },
          "value": { "type": "number", "description": "该时间点的数值" }
        }
      },
      "minItems": 4,
      "maxItems": 12
    }
  }
}
```

</details>

---

### PieChart — 饼图

展示整体中各部分的占比。

![PieChart](chart-images/03-PieChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "PieChart",
      "props": {
        "chartData": {
          "type": "PieChart",
          "label": "市场份额分布",
          "segments": [
            { "label": "Apple", "value": 42 },
            { "label": "Samsung", "value": 28 },
            { "label": "Xiaomi", "value": 15 },
            { "label": "Others", "value": 15 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "饼图：用圆形扇区展示整体中各部分的占比关系",
  "required": ["type", "label", "segments"],
  "properties": {
    "type": { "type": "string", "const": "PieChart" },
    "label": { "type": "string", "description": "图表标题" },
    "segments": {
      "type": "array",
      "description": "各扇区数据，每个扇区的面积占比由 value 相对总和决定",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "扇区名称" },
          "value": { "type": "number", "minimum": 0, "description": "扇区数值，所有扇区的 value 之和代表整体" }
        }
      },
      "minItems": 3,
      "maxItems": 6
    }
  }
}
```

</details>

---

### StatCallout — 数据卡片

突出展示单个关键指标。

![StatCallout](chart-images/04-StatCallout.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "StatCallout",
      "props": {
        "chartData": {
          "type": "StatCallout",
          "label": "总融资额",
          "value": "$47.5B",
          "comparison": "+23% vs 2023"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "数据卡片：大字号突出展示单个关键指标，可附带对比说明",
  "required": ["type", "label", "value"],
  "properties": {
    "type": { "type": "string", "const": "StatCallout" },
    "label": { "type": "string", "description": "指标名称" },
    "value": { "type": "string", "description": "指标值，含单位的格式化文本，如 '$47.5B'" },
    "comparison": { "type": "string", "description": "对比说明文本，如 '+23% vs 2023'" }
  }
}
```

</details>

---

### StatGrid — 数据网格

同时展示多个关键指标。

![StatGrid](chart-images/05-StatGrid.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "StatGrid",
      "props": {
        "chartData": {
          "type": "StatGrid",
          "label": "公司核心指标",
          "stats": [
            { "value": "$12.5B", "label": "市值" },
            { "value": "23.5%", "label": "毛利率" },
            { "value": "1,280", "label": "员工数" },
            { "value": "4.2x", "label": "P/E 比率" }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "数据网格：以卡片网格形式同时展示多个关键指标",
  "required": ["type", "label", "stats"],
  "properties": {
    "type": { "type": "string", "const": "StatGrid" },
    "label": { "type": "string", "description": "网格整体标题" },
    "stats": {
      "type": "array",
      "description": "各指标卡片，每张卡片展示一个 label-value 对",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "指标名称" },
          "value": { "type": "string", "description": "指标值，含单位的格式化文本" }
        }
      },
      "minItems": 2,
      "maxItems": 6
    }
  }
}
```

</details>

---

## 对比分析类

### ComparisonBar — 前后对比

展示两个状态的前后对比。

![ComparisonBar](chart-images/06-ComparisonBar.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "ComparisonBar",
      "props": {
        "chartData": {
          "type": "ComparisonBar",
          "label": "用户留存率变化",
          "before": { "label": "优化前", "value": 32 },
          "after": { "label": "优化后", "value": 58 },
          "unit": "%"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "前后对比：用两根柱子并排展示同一指标在两个状态下的数值变化",
  "required": ["type", "label", "before", "after"],
  "properties": {
    "type": { "type": "string", "const": "ComparisonBar" },
    "label": { "type": "string", "description": "对比主题标题" },
    "unit": { "type": "string", "description": "数值单位" },
    "before": {
      "type": "object",
      "description": "变化前的状态，渲染为左侧柱子",
      "required": ["label", "value"],
      "properties": {
        "label": { "type": "string", "description": "前状态名称，如 '优化前'" },
        "value": { "type": "number", "description": "前状态数值" }
      }
    },
    "after": {
      "type": "object",
      "description": "变化后的状态，渲染为右侧柱子",
      "required": ["label", "value"],
      "properties": {
        "label": { "type": "string", "description": "后状态名称，如 '优化后'" },
        "value": { "type": "number", "description": "后状态数值" }
      }
    }
  }
}
```

</details>

---

### VersusChart — 对决对比

两个实体在多个维度的全面对比。

![VersusChart](chart-images/07-VersusChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "VersusChart",
      "props": {
        "chartData": {
          "type": "VersusChart",
          "label": "Tesla vs BYD 对比",
          "entityA": { "name": "Tesla", "ticker": "TSLA" },
          "entityB": { "name": "BYD", "ticker": "BYD" },
          "metrics": [
            { "label": "营收", "valueA": 96.7, "valueB": 84.9, "unit": "$B" },
            { "label": "交付量", "valueA": 1.8, "valueB": 3.0, "unit": "M" },
            { "label": "毛利率", "valueA": 18.2, "valueB": 20.1, "unit": "%" }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "对决对比：两个实体在多个维度上逐项 PK，左右对称展示",
  "required": ["type", "label", "entityA", "entityB", "metrics"],
  "properties": {
    "type": { "type": "string", "const": "VersusChart" },
    "label": { "type": "string", "description": "对比主题标题" },
    "entityA": {
      "type": "object",
      "description": "左侧实体信息",
      "required": ["name"],
      "properties": {
        "name": { "type": "string", "description": "实体名称" },
        "ticker": { "type": "string", "description": "股票代码等简称标识" }
      }
    },
    "entityB": {
      "type": "object",
      "description": "右侧实体信息",
      "required": ["name"],
      "properties": {
        "name": { "type": "string", "description": "实体名称" },
        "ticker": { "type": "string", "description": "股票代码等简称标识" }
      }
    },
    "metrics": {
      "type": "array",
      "description": "对比维度列表，每行展示一个维度的左右数值对比",
      "items": {
        "type": "object",
        "required": ["label", "valueA", "valueB"],
        "properties": {
          "label": { "type": "string", "description": "维度名称" },
          "valueA": { "type": "number", "description": "entityA 在该维度的数值" },
          "valueB": { "type": "number", "description": "entityB 在该维度的数值" },
          "unit": { "type": "string", "description": "该维度的单位" }
        }
      },
      "minItems": 3,
      "maxItems": 6
    }
  }
}
```

</details>

---

### DumbbellChart — 哑铃图

展示多个类别的范围变化。

![DumbbellChart](chart-images/15-DumbbellChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "DumbbellChart",
      "props": {
        "chartData": {
          "type": "DumbbellChart",
          "label": "各部门薪资范围 (万元/年)",
          "dumbbellPoints": [
            { "label": "工程", "start": 30, "end": 80 },
            { "label": "产品", "start": 25, "end": 65 },
            { "label": "设计", "start": 20, "end": 55 },
            { "label": "运营", "start": 18, "end": 45 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "哑铃图：每行一个类别，用两端圆点和连线展示从 start 到 end 的范围跨度",
  "required": ["type", "label", "dumbbellPoints"],
  "properties": {
    "type": { "type": "string", "const": "DumbbellChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "数值单位" },
    "dumbbellPoints": {
      "type": "array",
      "description": "各类别的范围数据，每项渲染为一行哑铃",
      "items": {
        "type": "object",
        "required": ["label", "start", "end"],
        "properties": {
          "label": { "type": "string", "description": "类别名称" },
          "start": { "type": "number", "description": "范围起点值（左侧圆点）" },
          "end": { "type": "number", "description": "范围终点值（右侧圆点）" }
        }
      },
      "minItems": 2,
      "maxItems": 8
    }
  }
}
```

</details>

---

### LollipopChart — 棒棒糖图

突出特定项的对比。

![LollipopChart](chart-images/14-LollipopChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "LollipopChart",
      "props": {
        "chartData": {
          "type": "LollipopChart",
          "label": "各产品线满意度评分",
          "lollipopItems": [
            { "label": "产品 A", "value": 92, "highlight": true },
            { "label": "产品 B", "value": 85 },
            { "label": "产品 C", "value": 78 },
            { "label": "产品 D", "value": 71 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "棒棒糖图：用细线+顶端圆点展示各项数值，比柱状图更轻量，适合突出高亮项",
  "required": ["type", "label", "lollipopItems"],
  "properties": {
    "type": { "type": "string", "const": "LollipopChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "数值单位" },
    "lollipopItems": {
      "type": "array",
      "description": "各项数据，每项渲染为一根细线加顶端圆点",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "项目名称" },
          "value": { "type": "number", "description": "数值，决定棒棒糖的高度" },
          "highlight": { "type": "boolean", "default": false, "description": "是否高亮突出此项" }
        }
      },
      "minItems": 3,
      "maxItems": 8
    }
  }
}
```

</details>

---

## 排名与分布类

### RankingBar — 排行榜

有序排名展示。

![RankingBar](chart-images/09-RankingBar.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "RankingBar",
      "props": {
        "chartData": {
          "type": "RankingBar",
          "label": "全球 AI 芯片市场份额",
          "items": [
            { "label": "NVIDIA", "value": 80, "unit": "%" },
            { "label": "AMD", "value": 10, "unit": "%" },
            { "label": "Intel", "value": 5, "unit": "%" },
            { "label": "Others", "value": 5, "unit": "%" }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "排行榜：按数值从大到小排列的水平条形图，突出排名顺序",
  "required": ["type", "label", "items"],
  "properties": {
    "type": { "type": "string", "const": "RankingBar" },
    "label": { "type": "string", "description": "排行榜标题" },
    "items": {
      "type": "array",
      "description": "排名项目列表，按 value 从大到小排列",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "排名项名称" },
          "value": { "type": "number", "description": "排名数值，决定条形长度" },
          "unit": { "type": "string", "description": "该项的单位" }
        }
      },
      "minItems": 3,
      "maxItems": 10
    }
  }
}
```

</details>

---

### FunnelChart — 漏斗图

展示转化漏斗。

![FunnelChart](chart-images/13-FunnelChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "FunnelChart",
      "props": {
        "chartData": {
          "type": "FunnelChart",
          "label": "电商转化漏斗",
          "funnelStages": [
            { "label": "浏览商品", "value": 50000 },
            { "label": "加入购物车", "value": 12000 },
            { "label": "开始结算", "value": 5600 },
            { "label": "完成支付", "value": 3200 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "漏斗图：从上到下逐级收窄，展示各阶段的转化流失过程",
  "required": ["type", "label", "funnelStages"],
  "properties": {
    "type": { "type": "string", "const": "FunnelChart" },
    "label": { "type": "string", "description": "漏斗标题" },
    "funnelStages": {
      "type": "array",
      "description": "漏斗各阶段，从上到下按转化顺序排列，value 应逐级递减",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "阶段名称" },
          "value": { "type": "number", "minimum": 0, "description": "该阶段的数量，决定漏斗该层的宽度" }
        }
      },
      "minItems": 3,
      "maxItems": 6
    }
  }
}
```

</details>

---

### TreemapChart — 树图

展示层级结构中各部分的占比。

![TreemapChart](chart-images/16-TreemapChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "TreemapChart",
      "props": {
        "chartData": {
          "type": "TreemapChart",
          "label": "投资组合分布",
          "treemapItems": [
            { "label": "科技", "value": 45 },
            { "label": "医疗", "value": 20 },
            { "label": "金融", "value": 18 },
            { "label": "能源", "value": 10 },
            { "label": "消费", "value": 7 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "树图：用嵌套矩形展示各部分占整体的比例，面积越大占比越高",
  "required": ["type", "label", "treemapItems"],
  "properties": {
    "type": { "type": "string", "const": "TreemapChart" },
    "label": { "type": "string", "description": "图表标题" },
    "treemapItems": {
      "type": "array",
      "description": "各区块数据，value 越大矩形面积越大",
      "items": {
        "type": "object",
        "required": ["label", "value"],
        "properties": {
          "label": { "type": "string", "description": "区块名称，显示在矩形内" },
          "value": { "type": "number", "minimum": 0, "description": "区块数值，决定矩形面积占比" }
        }
      },
      "minItems": 4,
      "maxItems": 8
    }
  }
}
```

</details>

---

### BenchmarkHeatmap — 基准热力图

多实体多维度评分对比。

![BenchmarkHeatmap](chart-images/20-BenchmarkHeatmap.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "BenchmarkHeatmap",
      "props": {
        "chartData": {
          "type": "BenchmarkHeatmap",
          "label": "AI 模型能力对比",
          "dimensions": ["推理", "编码", "创意写作", "数学", "多语言"],
          "benchmarkEntities": [
            { "label": "GPT-4o", "scores": [92, 88, 90, 85, 91], "overall": 89 },
            { "label": "Claude 3.5", "scores": [90, 92, 88, 82, 87], "overall": 88 },
            { "label": "Gemini", "scores": [85, 80, 82, 88, 90], "overall": 85 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "基准热力图：用色块矩阵展示多个实体在多个维度的评分，颜色深浅表示分数高低",
  "required": ["type", "label", "dimensions", "benchmarkEntities"],
  "properties": {
    "type": { "type": "string", "const": "BenchmarkHeatmap" },
    "label": { "type": "string", "description": "图表标题" },
    "dimensions": {
      "type": "array",
      "description": "评分维度名称列表，作为热力图的列标题",
      "items": { "type": "string" },
      "minItems": 3,
      "maxItems": 6
    },
    "benchmarkEntities": {
      "type": "array",
      "description": "参与评分的实体列表，每个实体占热力图的一行",
      "items": {
        "$ref": "#/$defs/BenchmarkEntity"
      },
      "minItems": 3,
      "maxItems": 5
    }
  },
  "$defs": {
    "BenchmarkEntity": {
      "type": "object",
      "description": "一个被评分的实体，scores 数组与 dimensions 一一对应",
      "required": ["label", "scores", "overall"],
      "properties": {
        "label": { "type": "string", "description": "实体名称，显示在行标题" },
        "scores": {
          "type": "array",
          "description": "各维度的评分，顺序与 dimensions 一一对应，分数越高色块越深",
          "items": { "type": "number" }
        },
        "overall": { "type": "number", "description": "综合评分，显示在行末" }
      }
    }
  }
}
```

</details>

---

## 流程与关系类

### FlowDiagram — 流程图

展示线性流程或步骤序列。

![FlowDiagram](chart-images/11-FlowDiagram.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "FlowDiagram",
      "props": {
        "chartData": {
          "type": "FlowDiagram",
          "label": "用户注册流程",
          "stages": [
            { "label": "访问落地页", "stat": "10,000 UV" },
            { "label": "点击注册", "sublabel": "转化率 32%", "stat": "3,200" },
            { "label": "填写信息", "sublabel": "完成率 78%", "stat": "2,496" },
            { "label": "注册完成", "stat": "2,271" }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "流程图：用箭头串联的步骤节点，展示线性流程或操作序列",
  "required": ["type", "label", "stages"],
  "properties": {
    "type": { "type": "string", "const": "FlowDiagram" },
    "label": { "type": "string", "description": "流程标题" },
    "stages": {
      "type": "array",
      "description": "按顺序排列的流程步骤，前后步骤之间用箭头连接",
      "items": {
        "type": "object",
        "required": ["label"],
        "properties": {
          "label": { "type": "string", "description": "步骤名称" },
          "sublabel": { "type": "string", "description": "步骤补充说明，如转化率" },
          "stat": { "type": "string", "description": "步骤关键数据，如 '3,200'" }
        }
      },
      "minItems": 2,
      "maxItems": 6
    }
  }
}
```

</details>

---

### TimelineDiagram — 时间线

展示事件的时间顺序。

![TimelineDiagram](chart-images/12-TimelineDiagram.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "TimelineDiagram",
      "props": {
        "chartData": {
          "type": "TimelineDiagram",
          "label": "产品发布里程碑",
          "events": [
            { "date": "2024-Q1", "label": "MVP 上线", "impact": "首批 1000 用户" },
            { "date": "2024-Q2", "label": "A 轮融资", "impact": "$5M" },
            { "date": "2024-Q3", "label": "国际化上线", "impact": "覆盖 5 个市场" },
            { "date": "2025-Q1", "label": "突破 10 万用户" }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "时间线：沿时间轴排列的事件节点，展示关键里程碑的先后顺序",
  "required": ["type", "label", "events"],
  "properties": {
    "type": { "type": "string", "const": "TimelineDiagram" },
    "label": { "type": "string", "description": "时间线标题" },
    "events": {
      "type": "array",
      "description": "按时间顺序排列的事件列表，每个事件渲染为时间轴上的一个节点",
      "items": {
        "type": "object",
        "required": ["date", "label"],
        "properties": {
          "date": { "type": "string", "description": "事件时间标记，如 '2024-Q1'" },
          "label": { "type": "string", "description": "事件名称" },
          "impact": { "type": "string", "description": "事件影响或成果说明" }
        }
      },
      "minItems": 2,
      "maxItems": 8
    }
  }
}
```

</details>

---

### SankeyChart — 桑基图

展示流量或资金在节点之间的流向。

![SankeyChart](chart-images/17-SankeyChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "SankeyChart",
      "props": {
        "chartData": {
          "type": "SankeyChart",
          "label": "营收来源与支出分配",
          "nodes": [
            { "id": "ads", "label": "广告收入" },
            { "id": "sub", "label": "订阅收入" },
            { "id": "rd", "label": "研发" },
            { "id": "mkt", "label": "营销" },
            { "id": "ops", "label": "运营" }
          ],
          "links": [
            { "source": "ads", "target": "rd", "value": 30 },
            { "source": "ads", "target": "mkt", "value": 25 },
            { "source": "sub", "target": "rd", "value": 20 },
            { "source": "sub", "target": "ops", "value": 15 }
          ],
          "unit": "$M"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "桑基图：用不同宽度的流线连接节点，展示流量或资金在来源与去向之间的分配",
  "required": ["type", "label", "nodes", "links"],
  "properties": {
    "type": { "type": "string", "const": "SankeyChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "流量单位" },
    "nodes": {
      "type": "array",
      "description": "所有节点（来源和去向），每个节点渲染为一个方块",
      "items": {
        "type": "object",
        "required": ["id", "label"],
        "properties": {
          "id": { "type": "string", "description": "节点唯一标识，供 links 中 source/target 引用" },
          "label": { "type": "string", "description": "节点显示名称" }
        }
      },
      "minItems": 4,
      "maxItems": 8
    },
    "links": {
      "type": "array",
      "description": "节点之间的流向连线，线的宽度由 value 决定",
      "items": {
        "type": "object",
        "required": ["source", "target", "value"],
        "properties": {
          "source": { "type": "string", "description": "来源节点 id" },
          "target": { "type": "string", "description": "去向节点 id" },
          "value": { "type": "number", "minimum": 0, "description": "流量数值，决定连线宽度" }
        }
      }
    }
  }
}
```

</details>

---

### NetworkGraph — 网络关系图

展示实体之间的关系网络。

![NetworkGraph](chart-images/21-NetworkGraph.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "NetworkGraph",
      "props": {
        "chartData": {
          "type": "NetworkGraph",
          "label": "AI 行业竞争格局",
          "networkNodes": [
            { "id": "openai", "label": "OpenAI", "tier": 1, "type": "company", "highlight": true },
            { "id": "msft", "label": "Microsoft", "tier": 1, "type": "company" },
            { "id": "google", "label": "Google", "tier": 1, "type": "company" },
            { "id": "anthropic", "label": "Anthropic", "tier": 2, "type": "company" }
          ],
          "networkEdges": [
            { "from": "msft", "to": "openai", "type": "invests", "label": "$13B", "strength": 3 },
            { "from": "google", "to": "anthropic", "type": "invests", "label": "$2B", "strength": 2 },
            { "from": "openai", "to": "google", "type": "competes", "strength": 3 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "网络关系图：用节点和连线展示实体之间的关系网络，节点大小和边的粗细表示层级和强度",
  "required": ["type", "label", "networkNodes", "networkEdges"],
  "properties": {
    "type": { "type": "string", "const": "NetworkGraph" },
    "label": { "type": "string", "description": "图表标题" },
    "networkNodes": {
      "type": "array",
      "description": "网络中的实体节点，每个节点渲染为一个圆形",
      "items": {
        "$ref": "#/$defs/NetworkNode"
      },
      "minItems": 3,
      "maxItems": 8
    },
    "networkEdges": {
      "type": "array",
      "description": "节点之间的关系连线",
      "items": {
        "$ref": "#/$defs/NetworkEdge"
      }
    }
  },
  "$defs": {
    "NetworkNode": {
      "type": "object",
      "description": "一个网络节点，代表一个实体",
      "required": ["id", "label"],
      "properties": {
        "id": { "type": "string", "description": "节点唯一标识，供 edges 中 from/to 引用" },
        "label": { "type": "string", "description": "节点显示名称" },
        "tier": { "type": "integer", "description": "层级，1 为核心层，数字越大越外围，影响节点大小" },
        "type": { "type": "string", "description": "节点类别，如 'company'、'product'" },
        "highlight": { "type": "boolean", "default": false, "description": "是否高亮此节点" }
      }
    },
    "NetworkEdge": {
      "type": "object",
      "description": "两个节点之间的关系连线",
      "required": ["from", "to", "type", "strength"],
      "properties": {
        "from": { "type": "string", "description": "起始节点 id" },
        "to": { "type": "string", "description": "目标节点 id" },
        "type": { "type": "string", "description": "关系类型，如 'invests'、'competes'" },
        "label": { "type": "string", "description": "连线上的标注文本" },
        "strength": { "type": "integer", "minimum": 1, "maximum": 3, "description": "关系强度，1-3，决定连线粗细" }
      }
    }
  }
}
```

</details>

---

## 高级分析类

### WaterfallChart — 瀑布图

展示从起始值到最终值的累积变化。

![WaterfallChart](chart-images/08-WaterfallChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "WaterfallChart",
      "props": {
        "chartData": {
          "type": "WaterfallChart",
          "label": "利润构成分解",
          "items": [
            { "label": "总营收", "value": 100, "type": "total" },
            { "label": "成本", "value": -45, "type": "negative" },
            { "label": "运营费", "value": -20, "type": "negative" },
            { "label": "其他收入", "value": 8, "type": "positive" },
            { "label": "净利润", "value": 43, "type": "result" }
          ],
          "unit": "$M"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "瀑布图：用浮动柱子展示从起始值经过增减变化到最终值的累积过程",
  "required": ["type", "label", "items"],
  "properties": {
    "type": { "type": "string", "const": "WaterfallChart" },
    "label": { "type": "string", "description": "图表标题" },
    "unit": { "type": "string", "description": "数值单位" },
    "items": {
      "type": "array",
      "description": "瀑布图各步骤，通常以 total 开始、result 结束，中间是 positive/negative 增减项",
      "items": {
        "type": "object",
        "required": ["label", "value", "type"],
        "properties": {
          "label": { "type": "string", "description": "步骤名称" },
          "value": { "type": "number", "description": "步骤数值，正数为增加，负数为减少" },
          "type": {
            "type": "string",
            "enum": ["total", "positive", "negative", "result"],
            "description": "步骤类型：total=起始基准, positive=增加项(绿色), negative=减少项(红色), result=最终结果"
          }
        }
      },
      "minItems": 3,
      "maxItems": 8
    }
  }
}
```

</details>

---

### GaugeChart — 仪表盘

展示某个指标在范围中的位置或进度。

![GaugeChart](chart-images/10-GaugeChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "GaugeChart",
      "props": {
        "chartData": {
          "type": "GaugeChart",
          "label": "服务器 CPU 使用率",
          "value": "73%",
          "max": 100,
          "context": "警戒线：85%"
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "仪表盘：用半圆弧形表盘展示单个指标在范围中的位置，类似汽车仪表盘",
  "required": ["type", "label", "value", "max"],
  "properties": {
    "type": { "type": "string", "const": "GaugeChart" },
    "label": { "type": "string", "description": "指标名称" },
    "value": { "type": "string", "description": "当前值，含单位的格式化文本，如 '73%'" },
    "max": { "type": "number", "description": "表盘最大刻度值，指针位置 = value / max" },
    "context": { "type": "string", "description": "补充说明，如阈值或警戒线信息" }
  }
}
```

</details>

---

### QuadrantMatrix — 四象限矩阵

在二维空间中定位多个实体。

![QuadrantMatrix](chart-images/18-QuadrantMatrix.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "QuadrantMatrix",
      "props": {
        "chartData": {
          "type": "QuadrantMatrix",
          "label": "产品优先级矩阵",
          "xAxis": { "label": "用户影响力" },
          "yAxis": { "label": "开发成本" },
          "quadrantLabels": ["高影响低成本", "高影响高成本", "低影响低成本", "低影响高成本"],
          "quadrantEntities": [
            { "label": "功能 A", "x": 80, "y": 20, "highlight": true },
            { "label": "功能 B", "x": 85, "y": 75 },
            { "label": "功能 C", "x": 30, "y": 15 },
            { "label": "功能 D", "x": 25, "y": 80 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "四象限矩阵：以十字线将平面分为四个象限，将实体按两个维度定位在散点图上",
  "required": ["type", "label", "xAxis", "yAxis", "quadrantLabels", "quadrantEntities"],
  "properties": {
    "type": { "type": "string", "const": "QuadrantMatrix" },
    "label": { "type": "string", "description": "矩阵标题" },
    "xAxis": {
      "type": "object",
      "description": "X 轴（水平轴）定义",
      "required": ["label"],
      "properties": { "label": { "type": "string", "description": "X 轴含义，如 '用户影响力'" } }
    },
    "yAxis": {
      "type": "object",
      "description": "Y 轴（垂直轴）定义",
      "required": ["label"],
      "properties": { "label": { "type": "string", "description": "Y 轴含义，如 '开发成本'" } }
    },
    "quadrantLabels": {
      "type": "array",
      "description": "四个象限的名称，顺序：右上、左上、右下、左下",
      "items": { "type": "string" },
      "minItems": 4,
      "maxItems": 4
    },
    "quadrantEntities": {
      "type": "array",
      "description": "需要定位的实体列表，每个实体在二维平面上渲染为一个散点",
      "items": {
        "$ref": "#/$defs/QuadrantEntity"
      },
      "minItems": 3,
      "maxItems": 8
    }
  },
  "$defs": {
    "QuadrantEntity": {
      "type": "object",
      "description": "一个需要在四象限中定位的实体",
      "required": ["label", "x", "y"],
      "properties": {
        "label": { "type": "string", "description": "实体名称，显示在散点旁" },
        "x": { "type": "number", "minimum": 0, "maximum": 100, "description": "X 轴位置，0-100，50 为中线" },
        "y": { "type": "number", "minimum": 0, "maximum": 100, "description": "Y 轴位置，0-100，50 为中线" },
        "highlight": { "type": "boolean", "default": false, "description": "是否高亮此实体" }
      }
    }
  }
}
```

</details>

---

### SensitivityChart — 敏感性分析

展示一个变量变化对多个指标的影响程度。

![SensitivityChart](chart-images/19-SensitivityChart.png)

<details>
<summary>JSON 示例</summary>

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "SensitivityChart",
      "props": {
        "chartData": {
          "type": "SensitivityChart",
          "label": "利率敏感性分析",
          "inputLabel": "基准利率",
          "inputChange": "+100bps",
          "sensitivityEntities": [
            { "label": "房贷支出", "multiplier": 1.8 },
            { "label": "债券收益", "multiplier": 1.5 },
            { "label": "股票估值", "multiplier": -0.6 },
            { "label": "储蓄回报", "multiplier": 1.2 }
          ]
        }
      },
      "children": []
    }
  }
}
```

</details>

<details>
<summary>JSON Schema</summary>

```json
{
  "type": "object",
  "description": "敏感性分析：展示一个输入变量变化时，对多个输出指标的影响程度，正向影响向右、负向影响向左",
  "required": ["type", "label", "inputLabel", "inputChange", "sensitivityEntities"],
  "properties": {
    "type": { "type": "string", "const": "SensitivityChart" },
    "label": { "type": "string", "description": "图表标题" },
    "inputLabel": { "type": "string", "description": "变化的输入变量名称，如 '基准利率'" },
    "inputChange": { "type": "string", "description": "输入变量的变化幅度，如 '+100bps'" },
    "sensitivityEntities": {
      "type": "array",
      "description": "受影响的输出指标列表，每项渲染为一根双向条形",
      "items": {
        "type": "object",
        "required": ["label", "multiplier"],
        "properties": {
          "label": { "type": "string", "description": "受影响的指标名称" },
          "multiplier": { "type": "number", "description": "影响倍数，正值=正向影响，负值=负向影响" }
        }
      },
      "minItems": 2,
      "maxItems": 6
    }
  }
}
```

</details>
