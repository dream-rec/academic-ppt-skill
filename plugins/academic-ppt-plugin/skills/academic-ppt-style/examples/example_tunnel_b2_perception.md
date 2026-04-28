# 示例 — 长大隧道多模态感知（Template B 实例）

本示例演示了一个完整的需求→提示词→迭代过程，基于真实会话整理。任务：为"1.1.2 长大隧道感知方法"生成一张含"机器狗构成与控制 + 多模态协同采集 + 多模态语义表征"的学术汇报页。

---

## 1. 用户原始需求

> 生成一张隧道机器狗智能巡检（机器狗的构成与控制），对应右侧大标题 1.1 复杂空间数据采集，小标题为 1.1.2 长大隧道感知方法。传感器分为相机、雷达、气体，采集多模态数据（点云图像等）。四足机器人型号选为 unitree b2。红灰配色、白色背景，学术风。

## 2. 需求澄清（Agent 自问自答）

| 参数 | 值 |
|---|---|
| 主标题 | `1.1 复杂空间数据采集` |
| 子标题 | `1.1.2 长大隧道感知方法：（1）多模态感知` |
| 版式模板 | Template B（章节正文页 + 关键科学问题收束） |
| 模块 1 | 宇树 B2 四足平台与传感构成 |
| 模块 2 | 相机 / 激光雷达 / 气体多模态协同采集 |
| 模块 3 | 多模态点云-图像语义表征与数据产出 |
| 底部关键科学问题 | 是 |
| 底部文献行 | 否 |
| 硬件型号 | Unitree B2 · ZED 2 · Livox Mid-360 · 四合一气体阵列 · Jetson Orin |

## 3. 最终提示词（可直接复制到 Nano Banana / GPT-Image / Gemini-Image）

````text
Create a highly detailed professional academic presentation-style image in Simplified Chinese, red-gray color scheme on PURE WHITE background (#FFFFFF). Layout follows "Template B — top-left title + red-bar modules + key scientific question".

NO top full-width red banner. Title area at top-left with section pills at top-right of the title line.
Three modules use SOLID RED RECTANGULAR HEADER BARS (not rounded chips).
Bottom ends with a centered red bold "关键科学问题" sentence.
Draw exactly 7 labels around the B2 robot. Seven. Not eight.

================================
PAGE STRUCTURE (16:9, 3840×2160)
================================

— A) Title area (top-left), ~10% height, 60px outer margin:
   Main title (Bold, 44pt, #2C3E50): "1.1 复杂空间数据采集"
   Subtitle (Bold, 32pt, #2C3E50): "1.1.2 长大隧道感知方法：（1）多模态感知"
   Right side of the main title line (same baseline, right-aligned):
     • Pill 1 (active): SOLID RED #C0392B, white bold "多模态", 20pt, 4px corners
     • Pill 2 (inactive): LIGHT GRAY #BDC3C7, white bold "单模态", 20pt
   Far top-right corner: tiny 5-square strip (three red, two gray, 16×16px each) + 32×32 grayscale logo placeholder.

— B) Intro paragraph block, full width, ~10% height, 20pt #2C3E50, justified:
   "基于宇树 Unitree B2 四足平台构建长大隧道多模态感知体系，在弱光、高粉尘与 **GNSS 拒止** 环境下实现 **相机 / 激光雷达 / 气体** 数据的同步获取与时空对齐，支撑结构病害识别与施工场景理解。"
   (Render the ** ** markers as BOLD RED #C0392B inline keywords.)

— C) Three equal-width modules, ~65% height, 15px gutter, 60px outer margin.
   Module style:
     • SOLID RED HEADER BAR (#C0392B), ~56px height, 0–2px corners, white bold centered title 22pt.
     • Body: white fill, 1px #BDC3C7 border on left/right/bottom only, 16px padding, NO drop shadow.

— D) Bottom summary line (MANDATORY), full width, ~6% height, centered:
   "关键科学问题：GNSS 拒止空间下多模态数据的同步获取与时空一致性表征机理"
   Style: 26pt bold red #C0392B; thin 1px red underline below (60% page width, centered).

================================
MODULE 1 — Header: "宇树 B2 四足平台与传感构成"
================================
Upper 55% of module: one 3/4 side-perspective vector-style rendering of the Unitree B2 quadruped robot (industrial matte-black + dark-gray body, yellow accent lines, four articulated legs, top payload deck). Head faces LEFT, tail faces RIGHT. Subtle grid shadow on the ground.

===== STRICT CALLOUT CONSTRAINTS (READ AND OBEY) =====
Place EXACTLY 7 numbered callouts around the robot. FIXED CHECKLIST, each item appears EXACTLY ONCE, no synonyms, no duplicates, no empty boxes. After drawing, count boxes; if ≠ 7, remove extras.

Callout style: white fill, 1px #C0392B border, 4px corners, 13pt 思源黑体 dark-gray text; red "(n)" prefix; 1px red hairline leader with 4px red dot at anchor. Leader lines MUST NOT cross.

Fixed 7 callouts bound to anchor + spatial position:
  (1) 头部双目视觉相机（ZED 2）             → robot head front housing;    UPPER-LEFT
  (2) 三维激光雷达（Livox Mid-360，非重复扫描） → top-of-deck cylindrical puck; MIDDLE-LEFT
  (3) 顶部传感器扩展坞（Payload Bay）        → flat plate on robot back;    UPPER-RIGHT   [ONLY ONCE]
  (4) 气体检测阵列（CH₄ / CO / H₂S / O₂）    → module on side/rear of bay;  MIDDLE-RIGHT
  (5) IMU + 双频 RTK（隧道内退化为 LIO）     → sealed box inside torso;     LOWER-LEFT
  (6) 12 自由度关节伺服（3×4 配置）          → highlighted leg joint (dashed red circle); BOTTOM-CENTER
  (7) 边缘计算单元（NVIDIA Jetson Orin）     → rear compute box, separate from (3); LOWER-RIGHT  [ONLY ONCE]

Anti-duplication rules:
• "Payload Bay" belongs to (3) ONLY. Do NOT create a second Payload Bay label.
• "Jetson Orin" belongs to (7) ONLY. Do NOT create a second Jetson Orin label.
• "ZED 2" belongs to (1) ONLY. "Livox Mid-360" belongs to (2) ONLY.
• Total white-with-red-border callout boxes MUST equal 7.

Under the robot: ONE gray chip (light gray #ECF0F1, no red border, 12pt, NOT numbered):
   "IP67 防水防尘机体 / 8Ah 锂电池"

Lower 45% of module — "感知—决策—运动"三层控制架构图:
Three stacked rounded rectangles with red left accent bars + red down-arrows between them:
   Layer 1 — 感知层: 多模态传感器驱动 / PTP 时间同步 / 外参标定
   Layer 2 — 决策层: SLAM 定位（LIO-SAM）· 语义地图 · 路径规划（A*/Hybrid-A*）
   Layer 3 — 运动层: MPC 足端轨迹 · WBC 全身控制 · 步态切换（Trot / Stair）

Bottom spec strip, 2 cols × 3 rows, 12pt:
   尺寸 1098×450×645 mm | 连续负载 40 kg
   最大速度 6 m/s        | 续航 ≥4 h
   防护等级 IP67         | 控制周期 1 kHz

FINAL VERIFICATION for Module 1:
   ☑ Exactly 7 callout boxes, numbers (1)-(7) each once.
   ☑ No duplicate text; Payload Bay / Jetson Orin / ZED 2 / Livox Mid-360 each appear once.
   ☑ Gray IP67 chip appears once, not numbered.
   ☑ Leader lines do not cross.

================================
MODULE 2 — Header: "相机 / 激光雷达 / 气体多模态协同采集"
================================
Upper 50%: three equal-width vertical sub-cards, bottom-connected by a red fan-out line merging into a single "多模态数据总线" bar.

Sub-card 2-A — ZED 2:
• Realistic rendering of Stereolabs ZED 2 stereo camera (slim black aluminum bar, two round lenses, USB-C tail).
• Caption: "双目立体相机 ZED 2"
• Below: a tunnel-interior RGB thumbnail (warm headlamp light, horseshoe lining) with a couple of bounding-box annotations on shotcrete.
• Specs (12pt): 2×1920×1200 @ 30 fps · FoV 110° · 深度 0.3–20 m · 硬件触发同步

Sub-card 2-B — Livox Mid-360:
• Realistic rendering of Livox Mid-360 (black cylindrical puck, silver mirror aperture on top, short pigtail).
• Caption: "三维激光雷达 Livox Mid-360"
• Below: stylized non-repetitive flower/rose-petal scan pattern overlaid on a horseshoe tunnel cross-section, height-colored points (blue → cyan → yellow).
• Specs: 点频 200k pts/s · 探测距离 70 m · 垂直 FoV 59° · 精度 ±3 cm · 非重复扫描

Sub-card 2-C — 四合一气体传感阵列:
• Realistic rendering of a multi-gas detector module: black plastic enclosure, perforated metal grille on front with four labeled sensor head openings (CH₄, CO, H₂S, O₂), status LED, short RS-485 connector.
• Caption: "四合一气体传感阵列"
• Below: 2×2 grid of circular gauge chips (CH₄/CO/H₂S/O₂) with readings and green/yellow/red threshold arcs.
• Specs: 采样率 1 Hz · ppm 级精度 · 阈值可配置 · RS-485 输出

Lower 50% — 多模态同步采集时序图:
• Horizontal time axis, four parallel tracks (RGB, LiDAR, Gas, IMU) with colored pulse markers.
• Vertical red dashed line marking a synchronized instant labeled "PTP 硬件时间同步 · 对齐误差 < 1 ms".

Formula box (light red fill #FDEDEC, 1px red border):
   "多模态观测集:  Z_t = { I_t^{RGB},  P_t^{LiDAR},  g_t^{Gas},  x_t^{IMU} }"
   "同步约束:  |τ_i − τ_j| ≤ ε,   ε = 1 ms"

================================
MODULE 3 — Header: "多模态点云-图像语义表征与数据产出"
================================
Upper 55%: one large illustration of a long curved tunnel interior rendered as a dense colored 3D point cloud (warm near, cool far, horseshoe profile clearly visible). Overlay a semi-transparent RGB thumbnail in the lower-right with a thin red dashed camera frustum projecting onto the point cloud (pixel-level image-to-point-cloud registration). Highlight three semi-transparent red polygonal regions on the lining with small labels:
   "衬砌开裂区" · "渗漏水斑" · "施工缝"

Lower 45% — "多模态感知数据产出体系", three horizontal rows with red left accent bars:

Row 3-A  几何数据:
   • 全隧道三维点云地图（单次采集 ≥ 500 m）
   • 断面几何参数（拱顶高程 / 净宽 / 净空）
   • 数据体量：~2–5 GB / 100 m

Row 3-B  影像数据:
   • 高分辨率衬砌表观影像序列
   • 关键部位多视角图像集
   • 与点云像素级对齐

Row 3-C  环境 / 语义数据:
   • 沿程气体浓度时序曲线（CH₄ / CO / H₂S / O₂）
   • 语义标签集：{围岩, 衬砌, 设备, 人员, 病害}
   • 病害候选区自动标注

Bottom emphasis strip (red border, 1px, no fill), 14pt bold dark-gray:
   "核心贡献：GNSS 拒止、弱光、高粉尘隧道环境下的多模态数据同步获取与空间一致性表征"

================================
FOOTER
================================
NO footer row. NO citations, page numbers, author names. 40px bottom white margin BELOW the "关键科学问题" line.

================================
GLOBAL DESIGN SPECIFICATIONS
================================
• Resolution 3840×2160 (4K), 16:9, pure white #FFFFFF background only.
• Color palette: #C0392B / #FDEDEC / #2C3E50 / #7F8C8D / #BDC3C7 / #ECF0F1 plus point-cloud accents #2980B9 / #16A085 / #F1C40F.
• Typography: 思源黑体 (Source Han Sans SC). Main title 44pt Bold; subtitle 32pt Bold; module red headers 22pt Bold white; body 18–20pt; annotations 12–14pt; bottom "关键科学问题" 26pt Bold red.
• Module headers MUST be solid red rectangles with white centered Chinese text — NOT chip pills.
• Flat academic style: no drop shadows on modules; page background has no gradient.
• Intro paragraph uses inline red-bold highlights on 2 keywords only; do NOT red-highlight the whole sentence.
• Arrows: solid red #C0392B, 2–3px, triangular heads; robot callout lines 1px hairlines, no crossings.
• Hardware renderings (ZED 2 / Livox Mid-360 / 4-in-1 gas detector) must look like real products, at comparable visual weight.
• All Chinese must remain in simplified Chinese; math in standard italic.
• STRICTLY AVOID: top red banner; chip-style rounded module headers; duplicated sensor callouts; cartoon styling; colored page background; emoji; page numbers; citation rows.

Deliverable: one single 16:9 white-background image matching Template B — top-left title + 多模态/单模态 pills + short intro with 2 red-bold keywords + THREE modules with SOLID RED header bars ("宇树 B2 四足平台与传感构成" · "相机 / 激光雷达 / 气体多模态协同采集" · "多模态点云-图像语义表征与数据产出") + bottom red "关键科学问题" summary line. Content: 1.1.2 长大隧道感知方法 based on Unitree B2 + ZED 2 + Livox Mid-360 + 四合一气体阵列.
````

---

## 4. 多轮迭代记录（仅保留修订动作）

| 轮次 | 用户反馈 | 修订动作 |
|---|---|---|
| v1 → v2 | "气体要单独画传感器" + "相机/雷达要具体型号" + "删底部文献行" | 注入 `sensor_hardware_renderings.md` §4 的气体检测模块硬件渲染；硬件型号锁定 ZED 2 + Livox Mid-360；FOOTER 段写明 NO citations |
| v2 → v3 | "头部双目相机出现了两次（复读）" | 注入 `anti_duplication_block.md` 的强约束（固定清单 + 空间绑定 + ONLY ONCE 黑名单 + FINAL VERIFICATION） |
| v3 → v4 | "Payload Bay / Jetson Orin 又被画了两遍" | 对这两个关键词追加单独的 "belongs to (n) ONLY" 条款；在 prompt 顶部加 `Draw exactly 7 labels. Seven. Not eight.` |
| v4 → v5 | "换成 1.2 风格（左上标题、红条模块、关键科学问题）" | 从 Template A 切换到 Template B；标题变为左上、加药丸、底部强制"关键科学问题"行 |
| v5 → v6 | "第一段话和关键科学问题都再缩短" | 按 `ITERATION_CHECKLIST.md` §3.1 给出 A/B/C 三版长度选项，采用版本 B（~45 字）+ 问题 A（~25 字） |

---

## 5. 成品视觉对照

- 上半页：左上大小标题 + 右上"多模态/单模态"药丸 + 1 句 intro 含两处红色加粗关键词
- 中段：三个实心红条模块（B2 带 7 条引出线 / 三硬件卡 + 时序图 / 隧道点云 + 数据产出体系）
- 下半页：底部"关键科学问题"红色粗体单句 + 细红下划线
- 顶部 / 底部 / 背景：纯白
- 无页码、无文献行

---

## 6. 给新手的提示

- 首次生成时，**必须把 `anti_duplication_block.md` 完整嵌入**到涉及多标注的模块；否则复读概率 > 60%。
- 硬件只要提到了"相机 / 雷达 / 气体"三个词之一，**必须**配套引入 `sensor_hardware_renderings.md` 对应节。
- 若用户是工程评审汇报 → 倾向 Template B；若是首页 / 课题封面 → 倾向 Template A；若内容结构特殊 → 用 Template C 并显式声明 pattern。
