# Cost Box Plot Files in OMPL

## 问题 (Question)
这个里面有没有涉及绘制cost box plots的文件？

Are there any files related to drawing/plotting cost box plots in this repository?

## 答案 (Answer)
是的，OMPL 仓库中有多个文件涉及绘制 cost box plots。以下是详细说明：

Yes, there are multiple files in the OMPL repository that are related to drawing cost box plots. Here are the details:

---

## 1. 主要绘图文件 (Main Plotting Files)

### 1.1 `scripts/ompl_benchmark_statistics.py`

这是主要的基准测试统计脚本，用于生成包括 box plots 在内的各种图表。

This is the main benchmark statistics script that generates various plots including box plots.

**功能 (Features):**
- 解析基准测试日志文件 (Parses benchmark log files)
- 生成 SQLite 数据库 (Generates SQLite database)
- 为所有属性创建 box plots (Creates box plots for all attributes)
- 包括 cost 相关的 box plots (Includes cost-related box plots)

**关键代码段 (Key Code Section):**
```python
# Line 375 in ompl_benchmark_statistics.py
plt.boxplot(measurements, notch=0, sym='k+', vert=1, whis=1.5, bootstrap=1000)
```

**使用方法 (Usage):**
```bash
# 生成包含 box plots 的 PDF 文件
python scripts/ompl_benchmark_statistics.py logfile.log -d database.db -p plots.pdf
```

**Box plot 类型 (Types of Box Plots):**
- 时间 (time)
- 内存使用 (memory usage)
- 解决方案长度 (solution length)
- 解决方案平滑度 (solution smoothness)
- 解决方案间隙 (solution clearance)
- **最佳成本 (best cost)** - 来自 planner progress properties
- 其他整数和实数值属性 (Other integer and real-valued attributes)

---

### 1.2 `scripts/plannerarena/server.R`

这是 Planner Arena 的 R 服务器脚本，提供交互式 web 界面来可视化基准测试结果。

This is the R server script for Planner Arena, which provides an interactive web interface to visualize benchmark results.

**Box plot 代码位置 (Box Plot Code Locations):**
- Line 499: `geom_boxplot(outlier.shape = outlier.shape, na.rm = TRUE)`
- Line 543: `geom_boxplot(color = I("#3073ba"), fill = I("#99c9eb"), ...)`
- Line 560: `geom_boxplot(position = "dodge", outlier.shape = outlier.shape, ...)`

**功能 (Features):**
- 交互式 cost box plots (Interactive cost box plots)
- 支持多个 planner 的比较 (Supports comparison of multiple planners)
- 可选的对数刻度 (Optional logarithmic scale)
- 可选的离群值隐藏 (Optional outlier hiding)
- 简化前后的比较 (Before/after simplification comparison)

**使用方法 (Usage):**
```bash
# 本地运行 Planner Arena
plannerarena database.db
```

或访问在线版本 (Or visit the online version): http://plannerarena.org

---

## 2. Cost 追踪的实现 (Cost Tracking Implementation)

许多 planner 实现了 "best cost" 进度属性，这些数据会被收集并可以绘制成 box plots：

Many planners implement "best cost" progress properties, and this data is collected and can be plotted as box plots:

### 2.1 注册 Cost Progress Properties 的 Planner

以下 planner 注册了 "best cost" 属性：

The following planners register "best cost" properties:

| Planner | 文件 (File) | 代码行 (Line) |
|---------|-------------|---------------|
| RRTstar | `src/ompl/geometric/planners/rrt/src/RRTstar.cpp` | ~150 |
| RRTXstatic | `src/ompl/geometric/planners/rrt/src/RRTXstatic.cpp` | ~140 |
| TRRTstar | `src/ompl/geometric/planners/rrt/src/TRRTstar.cpp` | ~80 |
| LBTRRT | `src/ompl/geometric/planners/rrt/src/LBTRRT.cpp` | ~140 |
| LazyLBTRRT | `src/ompl/geometric/planners/rrt/src/LazyLBTRRT.cpp` | ~130 |
| BITstar | `src/ompl/geometric/planners/informedtrees/src/BITstar.cpp` | ~200 |
| AITstar | `src/ompl/geometric/planners/informedtrees/src/AITstar.cpp` | ~120 |
| EITstar | `src/ompl/geometric/planners/informedtrees/src/EITstar.cpp` | ~120 |
| BLITstar | `src/ompl/geometric/planners/lazyinformedtrees/src/BLITstar.cpp` | ~100 |
| PRM | `src/ompl/geometric/planners/prm/src/PRM.cpp` | ~250 |
| LazyPRM | `src/ompl/geometric/planners/prm/src/LazyPRM.cpp` | ~180 |
| SPARS | `src/ompl/geometric/planners/prm/src/SPARS.cpp` | ~200 |
| SPARStwo | `src/ompl/geometric/planners/prm/src/SPARStwo.cpp` | ~220 |
| SST | `src/ompl/geometric/planners/sst/src/SST.cpp` | ~150 |
| CForest | `src/ompl/geometric/planners/cforest/src/CForest.cpp` | ~140 |
| AnytimePathShortening | `src/ompl/geometric/planners/AnytimePathShortening.cpp` | ~80 |

### 2.2 代码示例 (Code Example)

```cpp
// 从 RRTstar.cpp
addPlannerProgressProperty("best cost REAL", [this] { return bestCostProperty(); });
```

这个属性会在 benchmark 运行期间定期记录，然后可以：
1. 绘制成时间序列图 (Plot as time series)
2. 聚合成 box plots 显示分布 (Aggregate into box plots to show distribution)

---

## 3. 文档和示例 (Documentation and Examples)

### 3.1 文档文件 (Documentation Files)

- **`doc/markdown/benchmark.md`** - 基准测试完整文档
  - 第 192 行解释 box plots (Line 192 explains box plots)
  - 包含 box plot 示例图片 (Includes example box plot images)
  - 说明 cost 追踪功能 (Explains cost tracking functionality)

### 3.2 示例代码 (Example Code)

- **`demos/PlannerProgressProperties.cpp`** - 演示如何使用 planner progress properties 进行基准测试
  - 展示如何收集 cost 数据 (Shows how to collect cost data)
  - 说明如何生成 plots (Explains how to generate plots)

---

## 4. 使用流程 (Usage Workflow)

### 4.1 完整工作流程 (Complete Workflow)

```bash
# 1. 运行基准测试 (Run benchmark)
./your_benchmark_program

# 2. 生成数据库和 box plots (Generate database and box plots)
python scripts/ompl_benchmark_statistics.py benchmark.log -d results.db -p plots.pdf

# 3. 查看 PDF 中的 box plots (View box plots in PDF)
# plots.pdf 将包含所有属性的 box plots，包括 cost
# plots.pdf will contain box plots for all attributes, including cost

# 4. 或使用 Planner Arena 进行交互式可视化 (Or use Planner Arena for interactive visualization)
plannerarena results.db
# 然后在浏览器中打开 http://localhost:5000
# Then open http://localhost:5000 in your browser
```

### 4.2 Box Plot 内容 (Box Plot Contents)

对于 cost 属性，box plot 显示：
- 中位数 (median)
- 四分位数 (quartiles)
- 异常值 (outliers)
- 最小/最大值 (min/max values)

每个 planner 一列，方便比较不同 planner 的 cost 性能。

---

## 5. 相关 Cost 属性 (Related Cost Attributes)

除了 "best cost" progress property，benchmark 还跟踪以下与 cost 相关的属性：

In addition to "best cost" progress property, benchmarks also track these cost-related attributes:

- **solution length** - 解决方案的长度，对于某些 optimization objectives 这就是 cost
- **solution clearance** - 解决方案的间隙，某些情况下用作 cost
- **solution smoothness** - 解决方案的平滑度
- **simplified solution length** - 简化后的解决方案长度

所有这些都会生成 box plots。

All of these generate box plots.

---

## 总结 (Summary)

**是的，OMPL 仓库包含完整的 cost box plot 绘制功能：**

**Yes, the OMPL repository contains complete cost box plot drawing functionality:**

1. ✅ Python 脚本用于生成 box plots (`ompl_benchmark_statistics.py`)
2. ✅ R 脚本用于交互式 box plots (`plannerarena/server.R`)
3. ✅ 多个 planner 注册 cost progress properties
4. ✅ 完整的文档和示例
5. ✅ 支持自动和手动 cost 数据收集

所有这些文件协同工作，提供全面的 cost 性能分析和可视化。

All these files work together to provide comprehensive cost performance analysis and visualization.
