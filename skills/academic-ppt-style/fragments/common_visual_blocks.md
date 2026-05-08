# Fragment — 可复用的学术视觉块

这里收录在学术 PPT 中**高频出现**的视觉组件。每个块都给出了可直接嵌入 prompt 的英文描述，可按需组合进 Template A / B / C 的任一模块。

---

## 1. 多模态同步采集时序图

```text
One horizontal timeline chart:
• X-axis = time (no explicit unit, just tick marks)
• 4 parallel tracks stacked vertically, labeled on the left: "RGB", "LiDAR", "Gas", "IMU"
• Each track has colored pulse markers at regular intervals (RGB: red, LiDAR: blue, Gas: green, IMU: orange)
• ONE vertical RED DASHED LINE crossing all four tracks to indicate a synchronized capture instant
• Below the dashed line, a small label: "PTP 硬件时间同步 · 对齐误差 < 1 ms"
• Below the timeline, a small formula box (light red fill #FDEDEC, 1px red border):
  "多模态观测集:  Z_t = { I_t^{RGB},  P_t^{LiDAR},  g_t^{Gas},  x_t^{IMU} }"
  "同步约束:  |τ_i − τ_j| ≤ ε,   ε = 1 ms"
```

## 2. "感知—决策—运动"三层控制架构图

```text
Three stacked rounded rectangles with red left accent bars (4px wide, #C0392B):
• Layer 1 (top) — 感知层: 多模态传感器驱动 / PTP 时间同步 / 外参标定
• Layer 2 (middle) — 决策层: SLAM 定位 · 语义地图 · 路径规划
• Layer 3 (bottom) — 运动层: MPC 足端轨迹 · WBC 全身控制 · 步态切换
• Red downward arrows between consecutive layers, centered horizontally
• Each layer: white fill, 1px #BDC3C7 border, 8px corners, 14pt dark-gray text
```

## 3. 技术路线纵向流水图

```text
Vertical workflow diagram with N stages, each stage = one rounded rectangle (600×140px equivalent):
• Stage title: 22pt bold, dark gray
• Left side: a small representative illustration (sensor / robot / data / formula)
• Right side: 3–5 bullet points summarizing the stage
• Between consecutive stages: THICK red downward arrow (#C0392B, triangular head) with a small text label on the shaft (e.g., "数据采集", "后端处理", "成果输出")
• Optional: small clock icon + time-estimate label next to each stage (e.g., "<5 min")
```

## 4. 结构质量监测 / 变形示意图

```text
Engineering-style tunnel cross-section with three deformation annotations:
• Horseshoe tunnel profile with concrete lining
• (a) 收敛变形: two BOLD RED HORIZONTAL ARROWS pointing inward from both sidewalls, labels "Δb₁" / "Δb₂", dashed reference line connecting them
  Inline formula: "水平收敛: Δb = Δb₁ + Δb₂", 精度 ±1mm
• (b) 拱顶沉降: BOLD RED VERTICAL ARROW pointing downward from the crown, label "Δh", dashed original-crown reference line
  Inline formula: "拱顶沉降: Δh = h₀ − h₁"
• (c) 周边位移: 8–12 small red arrows at key perimeter points (crown, springings, sidewalls, invert), labeled "Δd₁, Δd₂, ..."
  Red circular dots (M1, M2, ...) at each monitoring point, connecting dashed loop
• Legend box: red arrow = 变形方向 · red dot = 监测点位 · dashed line = 基准线
```

## 5. 三传感器对比三沙卡

```text
Three equal-width vertical sub-cards side-by-side, bottom-connected by a red fan-out line converging into a single "多模态数据总线" bar:
• Each card has:
  – Top: a REALISTIC HARDWARE RENDERING of the sensor (引用 sensor_hardware_renderings.md 的对应节)
  – Middle: a compact Chinese caption naming the sensor and model
  – Middle-lower: a small thumbnail visualization of the sensor's characteristic output (RGB image / LiDAR point cloud / gas reading gauges)
  – Bottom: a 12pt specs line (resolution / fps / FoV / accuracy / interface)
• Cards have 1px #BDC3C7 border, 8px corners, equal vertical height
```

## 6. 数据产出体系（三行制）

```text
Three horizontal sub-rows, each with a 4px red left accent bar and an inline section title:
• Row A — 几何数据:
    • 全隧道三维点云地图（单次 ≥ 500 m）
    • 断面几何参数（拱顶高程 / 净宽 / 净空）
    • 数据体量: ~2–5 GB / 100 m
• Row B — 影像数据:
    • 高分辨率衬砌表观影像序列
    • 关键部位多视角图像集
    • 与点云像素级对齐
• Row C — 环境 / 语义数据:
    • 沿程气体浓度时序曲线
    • 语义标签集 {围岩, 衬砌, 设备, 人员, 病害}
    • 病害候选区自动标注
```

## 7. 成果指标卡（5 卡纵列）

```text
Five vertically-stacked result-indicator cards, each card:
• Small icon on the left (32×32px)
• Metric name on top (bold 16pt)
• Large red number (36pt) with unit
• Status badge: green "正常范围" / yellow "预警" / red "超限"
• Small technical specs line below: "测量精度 / 预警阈值 / 报警阈值"
• Optional: mini trend sparkline (150×40px) at the bottom of the card
```

## 8. 风险等级饼图

```text
Small pie chart (120×120px) showing risk-level distribution of monitored cross-sections:
• Green sector "正常断面" (e.g., 85%)
• Yellow sector "预警断面" (e.g., 12%)
• Red sector "超限断面" (e.g., 3%)
• Percentages shown at each sector
• Legend below the pie with colored squares and counts
```

## 9. 核心贡献 / 核心技术强调条

```text
Full-width emphasis strip at the bottom of a module, 14–16pt bold dark-gray text:
• Light red fill #FDEDEC OR just a 1px red border with no fill
• Single-line statement starting with "核心贡献：..." or "核心技术：..."
• Optional leading red square bullet (8×8px)
```

## 10. 顶部标题右侧装饰条

```text
Tiny decorative element at the very top-right corner of the image:
• A strip of 5 small squares (16×16px, 6px gap) — three red #C0392B, two light gray #BDC3C7
• Immediately followed by a 32×32px grayscale institution-logo placeholder (thin outline only, no text)
• Total block sits at the same vertical position as the main title baseline
```

## 11. 数据流 / 算法管道块

```text
Horizontal pipeline with N nodes connected by red arrows:
• Each node = small rounded rectangle (white fill, 1px #BDC3C7, 8px corners), 24pt bold Chinese title
• Arrows: solid red #C0392B, 3px, triangular heads
• Inter-node spacing: 60–80px
• Optional mini icons inside each node (point-cloud symbol, camera symbol, brain-AI symbol, chart symbol)
```

## 12. 公式 / 表达式框

```text
Small inline formula box:
• Light red fill #FDEDEC, 1px red border #C0392B, 4px corners
• Padding 12px
• Contents: one or two formulas in standard italic math, 16pt
• Optional caption below the formula in 12pt dark gray
```

---

## 组合惯例

- **传感器对比卡（#5）+ 时序图（#1）** → 构成 Template A / B / C 的"多模态协同采集"模块
- **点云示意（sensor_hardware_renderings.md 第 9 节）+ 数据产出体系（#6）** → 构成"语义表征与数据产出"模块
- **技术路线纵向流水（#3）+ 成果指标卡（#7）** → 构成"技术实现流程"整页（适合 Template C 的 pattern C-4 左右分栏）
- **变形示意（#4）+ 风险饼图（#8）+ 成果指标卡（#7）** → 构成"结构质量监测成果"页
