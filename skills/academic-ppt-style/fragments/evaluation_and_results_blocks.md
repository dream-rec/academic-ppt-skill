# Fragment — 评测与结果展示可视化块

本 fragment 覆盖**学术论文 / 汇报成果页**中高频出现的**定量评测、消融、对比、可视化分析**等视觉组件。适用于 LLM / VLM / Agent / Embodied AI 任何领域的"成果展示页"。

---

## 1. Leaderboard 对比表

```text
Compact leaderboard table, minimal style:
• Header row (red #C0392B fill, white bold text): "Model · Params · Bench-1 · Bench-2 · Bench-3 · Bench-4 · Avg"
• N rows (8–12 models), each row:
    – Model name (left-aligned, bold for ours)
    – Param count (e.g., "7B", "70B")
    – Score columns, numeric; bold red for best, underline for second-best
    – Last column (Avg) with grayscale heatmap per cell
• Alternate row background: white / #F8F9FA for readability
• Our model row highlighted with a subtle red left-accent bar
• Bottom caption: "Bold = best, underlined = second-best. All results on <benchmark> at zero-shot."
```

## 2. 多维雷达图（Radar Chart）

```text
Hexagonal / Pentagonal / Octagonal radar chart:
• 5–8 axes labeled with capability dimensions (e.g., Reasoning, Knowledge, Math, Code, Instruction, Safety, Multilingual, Long-context)
• Axis scales normalized to 0–100
• 2–3 overlaid polygons with different colors:
    – Red (filled 20% opacity, stroke 2px): Ours
    – Dark Gray (dashed): Baseline A
    – Light Gray (dotted): Baseline B
• Vertex labels showing numeric scores
• Legend at bottom-right (small colored squares + model names)
• Title above chart (16pt bold): "Capability Profile"
```

## 3. 折线图（性能 vs 参数 / compute / 数据量）

```text
Line chart with log-scale X-axis:
• X-axis: model size (e.g., 1B → 70B) OR compute (FLOPs) OR training tokens
• Y-axis: benchmark score (linear, 0–100)
• Multiple curves with markers:
    – Red solid with circle markers: Our method
    – Gray dashed with triangle markers: Baseline 1
    – Gray dotted with square markers: Baseline 2
• Shaded confidence intervals (light color fill around each curve)
• Vertical annotation line at "Chinchilla-optimal" or a key checkpoint
• Grid: 1px light gray, both axes
• Frame: 1px #BDC3C7; title "Scaling Trend" top-left
```

## 4. 消融表（Ablation Table）

```text
Narrow vertical ablation table:
• Columns: Configuration · Component-A · Component-B · Component-C · Score
• Rows progressively add components:
    Row 1: Baseline — ✗ ✗ ✗ → 62.3
    Row 2: + Component A — ✓ ✗ ✗ → 65.1
    Row 3: + Component B — ✓ ✓ ✗ → 67.8
    Row 4: + Component C (Full) — ✓ ✓ ✓ → 71.5 (bold red)
• Red check ✓ and gray cross ✗ symbols for presence/absence
• Delta arrows between rows: "+2.8 / +2.7 / +3.7" in small red text
• Footer note: "每增加一个组件的相对增益"
```

## 5. 定性样例 (Qualitative Examples) 网格

```text
3×2 or 2×3 grid of qualitative examples:
• Each cell contains:
    – Input (image thumbnail / text prompt / state) at the top
    – Our model output (bottom-left) with green ✓ if correct, red ✗ if wrong
    – Baseline output (bottom-right) for comparison
• Differences highlighted with red underlines or colored boxes
• Each cell has a tiny case-ID label ("Case #1 · Counting")
• Optional small caption under each example explaining the insight
```

## 6. 柱状图对比（Grouped Bar Chart）

```text
Grouped bar chart:
• X-axis: categories / tasks (5-8 groups)
• Y-axis: metric value (e.g., accuracy %)
• Each group contains 3 bars side by side:
    – Red bar: Ours
    – Dark gray bar: Baseline A
    – Light gray bar: Baseline B
• Numeric labels on top of each bar
• Red highlight star ⭐ on bars where ours wins by >2%
• Legend top-right
• Y-axis grid lines; no chart junk
```

## 7. 热力图（Heatmap）

```text
Attention / correlation / confusion heatmap:
• Square grid N×N with row/column labels
• Color scale: white (low) → light red (mid) → deep red #C0392B (high)
• Numeric values inside each cell (optional, 10pt)
• Color bar on the right with legend "0.0 → 1.0"
• Use cases:
    – Attention map visualization
    – Confusion matrix
    – Task-task transfer matrix
    – Layer-wise probe accuracy
• Title above (bold 16pt)
```

## 8. 定量-定性二栏总览

```text
Two-column layout:
• Left column (40%) — 定量结果:
    Mini leaderboard table OR radar chart
    2-3 key numeric KPIs highlighted as large red numbers (36pt)
    e.g., "+7.2%  vs SOTA"  "3.5× Faster"
• Right column (60%) — 定性可视化:
    2×2 grid of qualitative examples
    Each with input thumbnail + our output + baseline output
• Bottom unified caption tying quant + qual findings
```

## 9. 指标卡片阵列（KPI Cards）

```text
Horizontal row of 4–5 large KPI cards:
• Each card: 240×160px, white fill, 1px #BDC3C7 border, red top accent bar
• Layout per card:
    – Icon top-left (32×32, red line-icon)
    – Metric label (14pt gray): "Success Rate"
    – Large number (42pt bold red): "87.3%"
    – Delta chip: "▲ +5.1" (green) or "▼ -1.2" (red)
    – Sub-note (12pt gray): "vs previous SOTA"
• Cards equally spaced with 15px gutters
• Used for page-top "成果速览"
```

## 10. 误差分析饼图 / Sankey

```text
Error breakdown visualization (choose one):
• Option A — Pie / donut chart:
    6 sectors, labeled with error types:
    "幻觉 30% · 指令跟随 20% · 推理错误 25% · OCR 失败 10% · 其他 15%"
    Sorted by size, largest first
• Option B — Sankey diagram:
    Left: input categories (Easy / Medium / Hard)
    Middle: model predictions (Correct / Wrong)
    Right: error types (Hallucination / Reasoning / OCR)
    Red flows for correct, gray flows for errors; flow width = count
• Side annotation: "N_total = 1000 samples · Error rate 15.8%"
```

## 11. 效率 / 延迟 / 吞吐对比

```text
Dual-axis or parallel-coordinate chart:
• X-axis: model names (bar chart style)
• Left Y-axis: accuracy (red bars)
• Right Y-axis: latency ms or throughput tok/s (gray line with markers)
• Each model has a red bar + a gray marker, showing quality-efficiency tradeoff
• Optional: small inset scatter plot with x=latency, y=accuracy and a red "Pareto Frontier" curve
• Bottom chip: "Pareto-optimal 模型用 ★ 标记"
```

## 12. 训练效率对比（收敛曲线）

```text
Multi-curve convergence chart:
• X-axis: training steps (or tokens / GPU-hours)
• Y-axis: evaluation metric (e.g., downstream accuracy)
• Several curves:
    – Red solid: Ours
    – Gray dashed: Baseline A
    – Gray dotted: Baseline B
• Horizontal dashed line at the baseline's final value, annotated with "Baseline Final Perf"
• Vertical dashed line where ours first reaches that level — annotated "3× Faster to reach"
• Legend + grid
```

## 13. 统计显著性 & 误差棒

```text
Bar chart with error bars:
• 4-6 bars (methods), each with:
    – Mean value (top of bar)
    – Black error bars (±1 std, from 3+ seeds)
    – Star above bar for statistical significance: * p<0.05, ** p<0.01, *** p<0.001
• Bracket connectors between compared bars with p-values labeled
• Caption: "Results averaged over N seeds; error bars ±1σ"
```

## 14. 人工评测（Human Evaluation）并排对比

```text
Bar or pie visualization of human preference outcomes:
• For each task, show three outcomes:
    – "Ours wins" (red bar / sector)
    – "Tie" (gray)
    – "Baseline wins" (dark gray)
• Percentages labeled on each segment
• Example: "Ours 62%  ·  Tie 18%  ·  Baseline 20%"
• Task names listed vertically on the left (Helpfulness / Harmlessness / Honesty / Accuracy)
• Bottom caption: "N = 500 prompts · 3 annotators · Fleiss κ = 0.67"
```

## 15. 失败案例集（Failure Cases）

```text
Compact 2×2 failure-case grid:
• Each cell:
    – Input at top
    – Our (wrong) output with red underline on the erroneous span
    – Ground truth answer below, green check
    – One-line diagnosis: "原因: <错误类型>"
• Bottom "经验教训" bar summarizing top 2-3 common failure patterns
• Style: white cards with light red border
```

---

## 组合建议

- **"成果速览页"（首页 KPI）** → #9 KPI Cards + #2 Radar + #11 Efficiency tradeoff
- **"主结果页"** → #1 Leaderboard + #3 Scaling curve + #5 Qualitative grid
- **"消融页"** → #4 Ablation + #13 Significance + #6 Grouped bars
- **"误差 / 失败分析页"** → #10 Error breakdown + #15 Failure cases + #7 Heatmap
- **"人评 & 偏好"页** → #14 Human Eval + #8 Quant-Qual two-column
- **"训练效率 / Pareto"页** → #11 Efficiency + #12 Convergence
