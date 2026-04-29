# Edgen 图表参考

> 所有图表类型的样式说明与使用场景。
>
> 提出新图表需求请参考：[chart-design-guide.md](./chart-design-guide.md)

---

## 图表类型总览

| 类别 | 图表类型 | 一句话描述 |
|---|---|---|
| **基础统计** | BarChart | 不同类别的数值比较 |
| | LineChart | 时间趋势变化 |
| | PieChart | 整体各部分占比 |
| | StatCallout | 突出单个关键指标 |
| | StatGrid | 同时展示多个关键指标 |
| **对比分析** | ComparisonBar | 两个状态的前后对比 |
| | VersusChart | 两实体多维度对比 |
| | DumbbellChart | 多类别范围变化 |
| | LollipopChart | 突出特定项的对比 |
| **排名与分布** | RankingBar | 有序排名 |
| | FunnelChart | 转化漏斗 |
| | TreemapChart | 层级占比 |
| | BenchmarkHeatmap | 多实体多维度评分 |
| **流程与关系** | FlowDiagram | 线性流程步骤 |
| | TimelineDiagram | 事件时间线 |
| | SankeyChart | 资金/流量流向 |
| | NetworkGraph | 实体关系网络 |
| **高级分析** | WaterfallChart | 累积增减变化 |
| | GaugeChart | 指标在范围中的位置 |
| | QuadrantMatrix | 二维空间定位 |
| | SensitivityChart | 变量变化影响分析 |

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

## 基础统计类

### BarChart — 柱状图

**大概样式：** 等宽柱子从左到右排列，每根柱子代表一个类别，高度代表数值大小。

![BarChart](chart-images/01-BarChart.png)

**使用场景：**
- "对比各季度营收"
- "各部门的预算分布"
- 不适合：有时间趋势关系 → 用 LineChart

---

### LineChart — 折线图

**大概样式：** 折线连接各时间点的数据，横轴是时间，纵轴是数值，可附带趋势总结文本。

![LineChart](chart-images/02-LineChart.png)

**使用场景：**
- "月活跃用户趋势"
- "收入的增长曲线"
- 不适合：类别之间没有时间顺序关系 → 用 BarChart

---

### PieChart — 饼图

**大概样式：** 圆形被分成若干扇区，每个扇区面积代表该部分在整体中的占比。

![PieChart](chart-images/03-PieChart.png)

**使用场景：**
- "市场份额分布"
- "收入来源结构"
- 不适合：比较绝对数值 → 用 BarChart；扇区超过 6 个 → 用 TreemapChart

---

### StatCallout — 数据卡片

**大概样式：** 大字号突出展示单个关键指标数值，下方可附带一行对比说明。

![StatCallout](chart-images/04-StatCallout.png)

**使用场景：**
- "总融资额是多少"
- "今天的 DAU"
- 不适合：同时展示多个指标 → 用 StatGrid

---

### StatGrid — 数据网格

**大概样式：** 多个指标卡片排成网格，每个卡片展示一个"指标名+数值"对。

![StatGrid](chart-images/05-StatGrid.png)

**使用场景：**
- "公司的核心指标概览"
- "一组关键 KPI 同时展示"
- 不适合：只有一个指标 → 用 StatCallout

---

## 对比分析类

### ComparisonBar — 前后对比

**大概样式：** 两根柱子并排，一根代表"之前"一根代表"之后"，直观展示同一指标的变化。

![ComparisonBar](chart-images/06-ComparisonBar.png)

**使用场景：**
- "优化前后的留存率对比"
- "改版前后的转化率变化"
- 不适合：多个维度对比 → 用 VersusChart

---

### VersusChart — 对决对比

**大概样式：** 左右对称布局，两个实体分列两侧，多个维度逐行对比，数值更优一方高亮。

![VersusChart](chart-images/07-VersusChart.png)

**使用场景：**
- "Tesla vs BYD 全面对比"
- "两个方案的优劣对比"
- 不适合：超过 2 个实体 → 用 BenchmarkHeatmap

---

### DumbbellChart — 哑铃图

**大概样式：** 每行一个类别，两端圆点加连线展示范围跨度（如薪资从最低到最高的区间）。

![DumbbellChart](chart-images/15-DumbbellChart.png)

**使用场景：**
- "各部门薪资范围"
- "各地区房价区间"
- 不适合：只关心单个值而非范围 → 用 BarChart

---

### LollipopChart — 棒棒糖图

**大概样式：** 与柱状图类似但更轻量——用细线加顶端圆点代替柱子，可高亮突出某一项。

![LollipopChart](chart-images/14-LollipopChart.png)

**使用场景：**
- "各产品线满意度评分"
- "突出某项指标在同类中的表现"
- 不适合：需要展示范围 → 用 DumbbellChart

---

## 排名与分布类

### RankingBar — 排行榜

**大概样式：** 水平条形图，从上到下按数值从大到小排列，第一名条形最长，附带排名序号。

![RankingBar](chart-images/09-RankingBar.png)

**使用场景：**
- "全球 AI 芯片市场份额排名"
- "销量 TOP 5"
- 不适合：项目之间没有排名关系 → 用 BarChart

---

### FunnelChart — 漏斗图

**大概样式：** 从上到下逐级收窄的梯形/倒三角，每层代表转化流程的一个阶段，宽度代表该阶段的数量。

![FunnelChart](chart-images/13-FunnelChart.png)

**使用场景：**
- "电商转化漏斗"
- "招聘流程各阶段人数"
- 不适合：各阶段没有递减关系 → 用 BarChart

---

### TreemapChart — 树图

**大概样式：** 嵌套矩形块填满整个区域，每块面积代表其在整体中的占比，颜色区分不同类别。

![TreemapChart](chart-images/16-TreemapChart.png)

**使用场景：**
- "投资组合分布"
- "各业务线收入占比"
- 不适合：项目少于 4 个 → 用 PieChart

---

### BenchmarkHeatmap — 基准热力图

**大概样式：** 表格式色块矩阵，行是实体、列是维度，颜色深浅代表评分高低，末列展示综合分。

![BenchmarkHeatmap](chart-images/20-BenchmarkHeatmap.png)

**使用场景：**
- "AI 模型能力多维度对比"
- "竞品多指标评分对比"
- 不适合：只有 2 个实体 → 用 VersusChart；想要图形化展示 → 用 RadarChart

---

## 流程与关系类

### FlowDiagram — 流程图

**大概样式：** 方框节点用箭头串联，从左到右（或从上到下）展示线性流程步骤，每个节点可附带统计数据。

![FlowDiagram](chart-images/11-FlowDiagram.png)

**使用场景：**
- "用户注册流程"
- "审批流程各步骤"
- 不适合：带时间维度 → 用 TimelineDiagram；有分支/汇合 → 暂不支持

---

### TimelineDiagram — 时间线

**大概样式：** 沿横向或纵向时间轴排列事件节点，每个节点标注时间、事件名和影响说明。

![TimelineDiagram](chart-images/12-TimelineDiagram.png)

**使用场景：**
- "产品发布里程碑"
- "公司发展历程"
- 不适合：无时间维度的线性步骤 → 用 FlowDiagram

---

### SankeyChart — 桑基图

**大概样式：** 左侧为来源节点、右侧为去向节点，之间用不同宽度的流线连接，线越粗代表流量越大。

![SankeyChart](chart-images/17-SankeyChart.png)

**使用场景：**
- "营收来源与支出分配"
- "用户从哪来、到哪去"
- 不适合：节点之间没有流向关系 → 用 NetworkGraph

---

### NetworkGraph — 网络关系图

**大概样式：** 圆形节点散布在平面上，节点之间用线条连接表示关系，节点大小表示重要程度，线条粗细表示关系强弱。

![NetworkGraph](chart-images/21-NetworkGraph.png)

**使用场景：**
- "AI 行业竞争格局"
- "公司之间的投资/合作关系"
- 不适合：有明确的流向 → 用 SankeyChart；是线性流程 → 用 FlowDiagram

---

## 高级分析类

### WaterfallChart — 瀑布图

**大概样式：** 浮动柱子展示从起始值经过增减到最终值的过程，增加项为绿色、减少项为红色，柱子之间有连接线。

![WaterfallChart](chart-images/08-WaterfallChart.png)

**使用场景：**
- "利润构成分解"
- "预算从哪增加、在哪消耗"
- 不适合：只关心最终结果而非过程 → 用 StatCallout

---

### GaugeChart — 仪表盘

**大概样式：** 半圆弧形表盘，指针指向当前值在范围中的位置，类似汽车仪表盘。

![GaugeChart](chart-images/10-GaugeChart.png)

**使用场景：**
- "服务器 CPU 使用率"
- "KPI 完成进度"
- 不适合：展示多个指标 → 用 StatGrid

---

### QuadrantMatrix — 四象限矩阵

**大概样式：** 十字线将平面分为四个象限，每个象限有标签名，实体以散点形式定位在二维空间中。

![QuadrantMatrix](chart-images/18-QuadrantMatrix.png)

**使用场景：**
- "产品优先级矩阵（影响力 vs 成本）"
- "竞品定位分析"
- 不适合：只有一个维度 → 用 RankingBar

---

### SensitivityChart — 敏感性分析

**大概样式：** 双向水平条形图，中线为零点，正向影响向右延伸、负向影响向左延伸，条形长度代表影响倍数。

![SensitivityChart](chart-images/19-SensitivityChart.png)

**使用场景：**
- "利率变化对各项资产的影响"
- "汇率敏感性分析"
- 不适合：关注绝对值而非变化影响 → 用 BarChart
