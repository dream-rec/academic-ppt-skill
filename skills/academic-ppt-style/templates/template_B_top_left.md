# Template B — 左上标题 + 红条模块 + 关键科学问题式

## 何时选择本模板

- **章节正文页**（非首页）
- **技术路线页 / 研究方法页**
- 需要强调页面收束语 **"关键科学问题：……"**
- 参考图特征：**没有顶部红色横幅**，标题在左上角，标题右侧有"二维/三维"或"多模态/单模态"这种药丸切换标签，三个模块标题是**实心红色矩形条**而非圆角芯片

## 页面结构骨架（从上到下）

```
┌──────────────────────────────────────────────────────────┐
│ 主标题 X.X 一级节              【红药丸 A】【灰药丸 B】   │   ~10%
│ 子标题 X.X.X 二级节：（N）某某方法                         │
├──────────────────────────────────────────────────────────┤
│ 短介绍段：一句话 + 行内【红色关键词1】【红色关键词2】       │   ~10%
├──────────────────────────────────────────────────────────┤
│ ▇▇ 模块1标题（红底白字实心矩形条） ▇▇                     │
│ ├────────────────┬──────────────────┬────────────────┤ │
│ │ 模块1内容       │ 模块2内容         │ 模块3内容       │ │  ~65%
│ │                │                  │                │ │
│ ├─ 模块2标题 ────┤                  ├────────────────┤ │
│ │                │                  │                │ │
│ └─ 模块3标题 ────┴──────────────────┴────────────────┘ │
├──────────────────────────────────────────────────────────┤
│         关键科学问题：XXXXXXXXXXXXXXXXXXXXXXX              │   ~6%
│         ─────────────────────── (thin red underline)      │
└──────────────────────────────────────────────────────────┘
```

## 提示词骨架

```text
Create a highly detailed professional academic presentation-style image in Simplified Chinese, red-gray color scheme on PURE WHITE background (#FFFFFF). Layout follows "Template B — top-left title + red-bar modules + key scientific question":

NO top full-width red banner. Title area is at top-left with section tag pills at top-right of the title line.
Three modules use SOLID RED RECTANGULAR HEADER BARS (not rounded chips).
Bottom ends with a centered red bold "关键科学问题" sentence (required).

================================
PAGE STRUCTURE (16:9, 3840×2160)
================================

— A) Title area (top-left), ~10% height, 60px outer margin:
   Main title (Bold, 44pt, #2C3E50): "[主标题 X.X 一级节]"
   Subtitle (Bold, 32pt, #2C3E50): "[副标题 X.X.X 二级节：（N）xx方法]"
   Right side of the main title line (same baseline, right-aligned):
     • Two horizontal tag pills, rectangular with 4px corners, ~44px height:
       – Pill 1 (active): SOLID RED #C0392B background, white bold text "[当前模式，例如：多模态]", 20pt
       – Pill 2 (inactive): LIGHT GRAY #BDC3C7 background, white bold text "[对照模式，例如：单模态]", 20pt
   Far top-right corner: tiny 5-square decorative strip (three red, two gray, each 16×16 px) + 32×32 grayscale logo placeholder.

— B) Intro paragraph block, full width, ~10% height:
   One concise justified Chinese paragraph, 20pt #2C3E50, line-height 1.5. Render 2–3 inline keywords in BOLD RED (#C0392B), NOT the whole sentence:
   "[一句话介绍，包含 **关键词1** 与 **关键词2** 等高亮短语]"

— C) Three equal-width content modules, ~65% height, 15px gutter, 60px outer margin.
   Each module:
     • SOLID RED HEADER BAR (#C0392B), full module width, ~56px height, 0–2px corners (flat rectangle). White bold Chinese title centered, 22pt.
     • Body: white background, 1px #BDC3C7 border on left/right/bottom only, inner padding 16px, NO drop shadow.
     • Content: a large visual + compact caption + 2–4 red bullet annotations.

— D) Bottom summary line (MANDATORY), full width, ~6% height, centered:
   One red bold Chinese sentence, 26pt, #C0392B:
   "关键科学问题：[一句话科学问题凝练]"
   Thin 1px red underline below the sentence, centered, extending ~60% of page width.

================================
MODULE 1 — header bar title (white on red, centered):
   "[模块1标题]"
================================
[模块1 内容布局，如有引出线去重需求，注入 anti_duplication_block.md]

================================
MODULE 2 — header bar title:
   "[模块2标题]"
================================
[模块2 内容布局]

================================
MODULE 3 — header bar title:
   "[模块3标题]"
================================
[模块3 内容布局]

================================
FOOTER
================================
NO footer row. NO citations / page numbers / author names. Leave a clean 40px bottom white margin BELOW the "关键科学问题" line.

================================
GLOBAL DESIGN SPECIFICATIONS
================================
[复制粘贴 design_specifications.md 的关键约束]

Deliverable: one single 16:9 white-background image faithfully reproducing Template B (top-left title + right-side pills + short intro with 2–3 inline red keywords + THREE modules with SOLID RED header bars + bottom red "关键科学问题" summary line).
```

## 关键差异（相比 Template A）

| 维度 | Template A | Template B |
|---|---|---|
| 顶部横幅 | **有**（红色全幅） | **无** |
| 标题位置 | 居顶、含横幅 | 左上、无底色 |
| 右侧标签 | 无 | 有（药丸切换） |
| 摘要段长度 | ~4–5 行 | ~1–2 行 |
| 摘要段高亮 | 单个短句 | 2–3 个行内关键词 |
| 模块 header | 圆角芯片（8px 圆角） | 实心矩形条（0–2px） |
| 模块阴影 | 轻阴影 | **无阴影、纯扁平** |
| 底部收束 | footer 可选 / 核心贡献条 | **必有"关键科学问题"** |

## 使用注意

- **模块 header 必须实心红色矩形条**，不能误用圆角芯片。
- 药丸标签的"当前模式"用红、"对照模式"用灰，位置在主标题右侧同基线。
- 底部"关键科学问题"是本模板的灵魂，**不可省略**。若内容确实没有科学问题，请改用 Template A 或 Template C。
- 介绍段保持 1–2 行，不要超过 3 行；需要更长摘要时改用 Template A。
