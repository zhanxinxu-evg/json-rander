# Edgen 图表设计指南

> 面向产品经理：如何设计和定义一个新的图表类型

---

## 目录

1. [系统简介](#1-系统简介)
2. [新图表的设计流程](#2-新图表的设计流程)
3. [如何定义数据模型 (Schema)](#3-如何定义数据模型-schema)
4. [Schema 设计规范](#4-schema-设计规范)
5. [完整示例：设计 RadarChart](#5-完整示例设计-radarchart)
6. [图表参考示例](#6-图表参考示例)
7. [图表选型参考](#7-图表选型参考)

---

## 1. 系统简介

### 先了解两个概念

在进入正文之前，先用最简单的方式理解两个会反复出现的术语：

**JSON（数据格式）**

JSON 是一种通用的数据格式，类似于一张结构化的表单。用花括号 `{}` 表示一组信息，用方括号 `[]` 表示一个列表。

举个例子，一张柱状图的数据用 JSON 表达出来长这样：

```json
{
  "label": "各季度营收",
  "values": [
    { "label": "Q1", "value": 12.5 },
    { "label": "Q2", "value": 14.8 },
    { "label": "Q3", "value": 16.2 }
  ],
  "unit": "$B"
}
```

这就像一张表单：
- `"label"` 是图表标题
- `"values"` 是一个列表，每一项有一个标签名和一个数值
- `"unit"` 是数值的单位

**Schema（数据模型 / 数据结构定义）**

Schema 就是"这张表单应该长什么样"的规格说明——有哪些字段、每个字段填什么类型的值、哪些是必填的。

用生活中的类比：JSON 是一张**已经填好的表单**，Schema 是**空白表单的模板设计**。

你作为产品经理设计新图表时，核心工作就是设计这个 Schema——决定表单里应该有哪些格子、每个格子填什么内容。

### 工作原理（一句话版）

AI 在回复中输出一段图表数据 JSON → 前端自动识别并渲染为可视化图表。

### 产品经理需要做什么

当你需要新增一个图表类型时，你的核心职责是：

1. **明确图表用途** — 解决什么场景、展示什么信息
2. **设计 Schema** — 定义图表需要哪些数据字段、每个字段的类型和含义（类似设计一张表单模板）
3. **编写 AI 使用说明** — 告诉 AI 什么时候该用这个图表、怎么填充数据

你**不需要**关心：前端组件的实现、注册流程、动画效果等技术细节，这些由工程师完成。

### 图表数据的整体结构

所有图表的数据都遵循同一个外层格式，你只需要设计 `chartData` 内部的字段：

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "你的图表类型名",
      "props": {
        "chartData": {
          "type": "你的图表类型名",
          "label": "图表标题",
          ← 你设计的数据字段放这里
        }
      },
      "children": []
    }
  }
}
```

- `type`：图表类型名称，PascalCase 格式（如 `RadarChart`）
- `label`：图表标题，必填，每张图表都有

其余的 `root`、`elements`、`children` 等外层结构是固定的，不需要关心。

---

## 2. 新图表的设计流程

### 2.1 需求澄清

回答以下问题：

| 问题 | 填写 |
|---|---|
| 图表名称（英文 PascalCase） | 例：`RadarChart` |
| 一句话描述 | 例：在多个维度上对比一个或多个实体的能力分布 |
| 典型使用场景 | 例：竞品多维度能力对比、团队技能评估 |
| 与哪些示例图表相似？区别是什么？ | 例：类似 BenchmarkHeatmap 但用雷达网格展示 |
| 最少需要几个数据点 | 例：至少 3 个维度 |
| 建议数据量上限 | 例：不超过 8 个维度、3 个实体 |

### 2.2 设计 Schema

定义 `chartData` 需要的所有字段（详见第 3 节）。

### 2.3 编写 AI 使用说明

为 AI Agent 编写一行说明，格式如下：

```
| 图表类型名 | 适用场景（英文简述） | 关键字段简写 |
```

例如：

```
| RadarChart | Multi-axis comparison | radarAxes[], radarEntities[{label,values[],highlight?}] |
```

### 2.4 提供测试数据

写 1-2 个完整的 JSON 示例数据，方便工程师验证渲染效果。

---

## 3. 如何定义数据模型 (Schema)

简单说，设计 Schema 就是在设计一张空白表单：

- 表单里有哪些**格子**（字段）
- 每个格子应该填**什么类型**的内容（文本？数字？列表？）
- 哪些格子是**必须填**的，哪些可以留空

### 3.1 字段设计模板

对于 `chartData` 中你需要设计的每个字段，需要说明：

| 属性 | 说明 |
|---|---|
| **字段名** | camelCase 格式，建议带图表前缀 |
| **类型** | `string` / `number` / `boolean` / `string[]` / `对象数组` 等 |
| **是否必填** | 必填 or 可选 |
| **含义** | 简短描述字段的用途 |
| **约束** | 取值范围、格式要求等 |

### 3.2 常用字段类型

你在描述字段时，需要标注它的"类型"——也就是这个格子里应该填什么。以下是最常用的几种：

| 类型 | 通俗含义 | 示例 |
|---|---|---|
| `string` | 一段文本 | `"营收增长"` |
| `number` | 一个数字 | `42.5` |
| `boolean` | 是或否 | `true`（是）/ `false`（否） |
| `string[]` | 一组文本（列表） | `["推理", "编码", "创意"]` |
| `{label, value}` | 一组搭配信息（对象） | `{"label": "Q1", "value": 12.5}` |
| `{label, value}[]` | 多组搭配信息（对象列表） | 多个上述对象 |

> **小提示：** `[]` 表示"一组/多个"，`{}` 表示"一条包含多个属性的记录"，`?` 表示"可选"。

### 3.3 公共字段（所有图表都有）

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `type` | `string` | 是 | 图表类型名，与外层 type 一致 |
| `label` | `string` | 是 | 图表标题 |
| `unit` | `string` | 否 | 数值单位，如 `"$B"`, `"%"`, `"x"` |

### 3.4 unit 字段约定

| 格式 | 含义 | 显示效果 |
|---|---|---|
| `"$B"` | 十亿美元 | `$12.5B` |
| `"$M"` | 百万美元 | `$350M` |
| `"%"` | 百分比 | `42%` |
| `"x"` | 倍数 | `4.2x` |
| `"K"` | 千 | `120K` |

---

## 4. Schema 设计规范

### 4.1 字段命名规则

| 规则 | 正确示例 | 错误示例 |
|---|---|---|
| camelCase 命名 | `radarAxes` | `radar_axes`, `RadarAxes` |
| 带图表前缀避免冲突 | `radarEntities` | `entities`（与其他图表冲突） |
| 数组字段名用复数 | `radarAxes` | `radarAxis` |
| 布尔字段用形容词 | `highlight` | `isHighlight` |
| 可选标签/描述字段 | `sublabel`, `subtitle` | `desc`（太模糊） |

### 4.2 为什么要带图表前缀

所有图表类型共用一个 `ChartData` 数据结构。如果不同图表使用同名字段但含义不同，就会产生冲突。

以下是示例图表如何避免冲突的：

| 图表 | 主数据字段 | 如果不加前缀会怎样 |
|---|---|---|
| FunnelChart | `funnelStages` | 和 FlowDiagram 的 `stages` 冲突 |
| NetworkGraph | `networkNodes` / `networkEdges` | 和 SankeyChart 的 `nodes` / `links` 概念冲突 |
| QuadrantMatrix | `quadrantEntities` | 和 BenchmarkHeatmap 的 `benchmarkEntities` 冲突 |
| SensitivityChart | `sensitivityEntities` | 同上 |

**原则：** 如果你的字段结构（子字段含义或个数）与已有字段不同，必须加图表前缀。

可以复用已有字段的情况：如果新图表的数据结构与 `BarChart` 的 `values[{label, value}]` 完全一致，则可以直接用 `values`，无需新建字段。

### 4.3 自定义子类型的设计

当你的数据项有多个属性时，需要定义一个子类型。用表格描述即可：

**示例：RadarEntity（雷达图的实体）**

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `label` | `string` | 是 | 实体名称 |
| `values` | `number[]` | 是 | 各维度数值，与 radarAxes 一一对应 |
| `color` | `string` | 否 | 自定义颜色（HEX 格式，如 `"#FF0000"`） |
| `highlight` | `boolean` | 否 | 是否高亮显示此实体 |

### 4.4 数据量建议

每个图表 Schema 都应标注建议的数据量范围：

- **最少数据点**：低于此数量图表没有意义
- **建议范围**：最佳的展示效果
- **上限**：超过此数量会导致视觉拥挤

---

## 5. 完整示例：设计 RadarChart

以下是一个产品经理设计新图表的完整交付物示例：

---

### 图表名称

**RadarChart**（雷达图 / 蛛网图）

### 用途描述

在多个维度上对比一个或多个实体的能力分布，以雷达网格的形式直观展示各维度的强弱。

### 典型场景

- AI 模型多维度能力对比（推理、编码、创意、数学等）
- 竞品产品特性对比
- 团队成员技能评估
- 投资组合风险维度分析

### 与示例图表的区别

| 对比图表 | 区别 |
|---|---|
| BenchmarkHeatmap | Heatmap 是表格式的色块，适合实体较多；RadarChart 是图形化的雷达网格，更直观但实体数量受限 |
| VersusChart | Versus 限定两个实体一对一对比；RadarChart 支持 1-3 个实体叠加 |
| BarChart | BarChart 是一维的柱状对比；RadarChart 展示多维度的"形状"差异 |

### Schema 定义

#### chartData 专属字段

| 字段 | 类型 | 必填 | 说明 | 约束 |
|---|---|---|---|---|
| `radarAxes` | `string[]` | 是 | 维度名称列表 | 3-8 个 |
| `radarEntities` | `RadarEntity[]` | 是 | 实体列表 | 1-3 个 |

#### 子类型：RadarEntity

| 字段 | 类型 | 必填 | 说明 | 约束 |
|---|---|---|---|---|
| `label` | `string` | 是 | 实体名称 | — |
| `values` | `number[]` | 是 | 各维度数值 | 长度必须等于 radarAxes 长度 |
| `color` | `string` | 否 | 自定义颜色 | HEX 格式 |
| `highlight` | `boolean` | 否 | 是否高亮此实体 | 默认 false |

### 数据量建议

| 参数 | 最小 | 推荐 | 最大 |
|---|---|---|---|
| 维度数 | 3 | 5-6 | 8 |
| 实体数 | 1 | 2 | 3 |

### AI 使用说明（给 chart-middleware 用的一行）

```
| RadarChart | Multi-axis comparison | radarAxes[], radarEntities[{label,values[],highlight?}] |
```

### 测试数据 1：双实体对比

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "RadarChart",
      "props": {
        "chartData": {
          "type": "RadarChart",
          "label": "GPT-4o vs Claude 3.5 能力对比",
          "radarAxes": ["推理", "编码", "创意写作", "数学", "多语言", "指令遵循"],
          "radarEntities": [
            {
              "label": "GPT-4o",
              "values": [92, 88, 90, 85, 91, 89],
              "highlight": true
            },
            {
              "label": "Claude 3.5",
              "values": [90, 92, 88, 82, 87, 94]
            }
          ]
        }
      },
      "children": []
    }
  }
}
```

### 测试数据 2：单实体分析

```json
{
  "root": "chart",
  "elements": {
    "chart": {
      "type": "RadarChart",
      "props": {
        "chartData": {
          "type": "RadarChart",
          "label": "Tesla 竞争力分析",
          "radarAxes": ["品牌力", "技术实力", "成本控制", "渠道覆盖", "用户满意度"],
          "radarEntities": [
            {
              "label": "Tesla",
              "values": [95, 90, 60, 45, 82]
            }
          ]
        }
      },
      "children": []
    }
  }
}
```

---

## 6. 图表参考示例

以下是每种图表的完整 JSON 示例，可以直接参考或用于测试。

### 基础统计类

#### BarChart — 柱状图

展示不同类别的数值比较。

![BarChart](chart-images/01-BarChart.png)

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

#### LineChart — 折线图

展示时间趋势变化。

![LineChart](chart-images/02-LineChart.png)

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

#### PieChart — 饼图

展示整体中各部分的占比。

![PieChart](chart-images/03-PieChart.png)

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

#### StatCallout — 数据卡片

突出展示单个关键指标。

![StatCallout](chart-images/04-StatCallout.png)

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

#### StatGrid — 数据网格

同时展示多个关键指标。

![StatGrid](chart-images/05-StatGrid.png)

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

---

### 对比分析类

#### ComparisonBar — 前后对比

展示两个状态的前后对比。

![ComparisonBar](chart-images/06-ComparisonBar.png)

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

#### VersusChart — 对决对比

两个实体在多个维度的全面对比。

![VersusChart](chart-images/07-VersusChart.png)

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

#### DumbbellChart — 哑铃图

展示多个类别的范围变化。

![DumbbellChart](chart-images/15-DumbbellChart.png)

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

#### LollipopChart — 棒棒糖图

突出特定项的对比。

![LollipopChart](chart-images/14-LollipopChart.png)

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

---

### 排名与分布类

#### RankingBar — 排行榜

有序排名展示。

![RankingBar](chart-images/09-RankingBar.png)

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

#### FunnelChart — 漏斗图

展示转化漏斗。

![FunnelChart](chart-images/13-FunnelChart.png)

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

#### TreemapChart — 树图

展示层级结构中各部分的占比。

![TreemapChart](chart-images/16-TreemapChart.png)

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

#### BenchmarkHeatmap — 基准热力图

多实体多维度评分对比。

![BenchmarkHeatmap](chart-images/20-BenchmarkHeatmap.png)

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

---

### 流程与关系类

#### FlowDiagram — 流程图

展示线性流程或步骤序列。

![FlowDiagram](chart-images/11-FlowDiagram.png)

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

#### TimelineDiagram — 时间线

展示事件的时间顺序。

![TimelineDiagram](chart-images/12-TimelineDiagram.png)

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

#### SankeyChart — 桑基图

展示流量或资金在节点之间的流向。

![SankeyChart](chart-images/17-SankeyChart.png)

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

#### NetworkGraph — 网络关系图

展示实体之间的关系网络。

![NetworkGraph](chart-images/21-NetworkGraph.png)

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

---

### 高级分析类

#### WaterfallChart — 瀑布图

展示从起始值到最终值的累积变化。

![WaterfallChart](chart-images/08-WaterfallChart.png)

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

#### GaugeChart — 仪表盘

展示某个指标在范围中的位置或进度。

![GaugeChart](chart-images/10-GaugeChart.png)

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

#### QuadrantMatrix — 四象限矩阵

在二维空间中定位多个实体。

![QuadrantMatrix](chart-images/18-QuadrantMatrix.png)

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

#### SensitivityChart — 敏感性分析

展示一个变量变化对多个指标的影响程度。

![SensitivityChart](chart-images/19-SensitivityChart.png)

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

---

## 7. 图表选型参考

### 选型决策树

```
你想展示什么？
│
├─ 单个关键数字 → StatCallout
├─ 多个关键数字 → StatGrid
│
├─ 数值比较
│  ├─ 不同类别比较 → BarChart
│  ├─ 前后对比（2 值） → ComparisonBar
│  ├─ 两实体多维对比 → VersusChart
│  ├─ 范围变化 → DumbbellChart
│  └─ 突出特定项 → LollipopChart
│
├─ 趋势变化 → LineChart
├─ 累计增减 → WaterfallChart
│
├─ 占比分布
│  ├─ 简单占比 → PieChart
│  └─ 层级占比 → TreemapChart
│
├─ 排名排序 → RankingBar
├─ 转化漏斗 → FunnelChart
├─ 进度/比率 → GaugeChart
│
├─ 流程/时间线
│  ├─ 流程步骤 → FlowDiagram
│  └─ 事件时间线 → TimelineDiagram
│
├─ 流向/关系
│  ├─ 资金或值流向 → SankeyChart
│  └─ 实体关系网络 → NetworkGraph
│
└─ 多维度分析
   ├─ 二维定位 → QuadrantMatrix
   ├─ 敏感性分析 → SensitivityChart
   └─ 多维度评分 → BenchmarkHeatmap
```

### 场景推荐搭配

| 产品场景 | 推荐图表组合 |
|---|---|
| 财报分析 | StatCallout + BarChart + WaterfallChart |
| 竞品对比 | VersusChart 或 BenchmarkHeatmap |
| 用户增长 | LineChart + FunnelChart + StatGrid |
| 市场份额 | PieChart + RankingBar + TreemapChart |
| 行业研究 | NetworkGraph + TimelineDiagram + QuadrantMatrix |
| 投资分析 | SankeyChart + SensitivityChart + GaugeChart |

### 各图表建议数据量

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

## 附：产品经理交付物模板

设计新图表时，请复制以下模板并填写：

```markdown
## 图表名称：___Chart

### 用途
（一句话描述：这个图表用来展示什么信息）

### 典型场景
- 场景 1
- 场景 2

### 与示例图表的区别
| 对比图表 | 区别 |
|---|---|
| XXXChart | ... |

### Schema 定义

#### chartData 字段

| 字段 | 类型 | 必填 | 说明 | 约束 |
|---|---|---|---|---|
| `xxxField` | `string` | 是 | ... | ... |
| `xxxItems` | `XxxItem[]` | 是 | ... | ... |

#### 子类型：XxxItem

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `label` | `string` | 是 | ... |
| `value` | `number` | 是 | ... |

### 数据量建议

| 参数 | 最小 | 推荐 | 最大 |
|---|---|---|---|
| ... | ... | ... | ... |

### AI 使用说明

| 图表类型名 | 适用场景 | 关键字段 |
|---|---|---|
| `___Chart` | ... | ... |

### 测试数据

（完整 JSON 示例，可直接用于测试渲染效果）
```
