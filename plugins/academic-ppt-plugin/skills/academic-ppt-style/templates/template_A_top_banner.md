# Template A — 顶部红色横幅式

## 何时选择本模板

- 成果 / 章节 **首页** 或 **封面页**
- 需要突出"成果 N"、"课题 N"等章节归属
- 参考图有**贯穿页面顶部的红色横幅**
- 需要三面板 ①②③ 带**圆角芯片 header** 的排版

## 页面结构骨架（从上到下）

```
┌──────────────────────────────────────────────────────────┐
│ [红色横幅] 一、成果N: 章节大标题                           │   ~6%
├──────────────────────────────────────────────────────────┤
│ ▍主标题 X.X 一级节  ·  副标题 X.X.X 二级节                │   ~8%
├──────────────────────────────────────────────────────────┤
│ 摘要段：一段 justified 的正文，含【一个】红色高亮粗体短句   │   ~10%
├──────────┬──────────────────┬──────────────────────────┤
│ ① 面板1  │ ② 面板2          │ ③ 面板3                  │
│（圆角芯片│ 圆角芯片 header   │ 圆角芯片 header）         │  ~70%
│ header + │ + 技术插图 +      │ + 技术插图 +             │
│ 技术插图）│   下部补充块       │   下部补充块              │
└──────────┴──────────────────┴──────────────────────────┘
                                                              ~6% bottom margin
```

## 提示词骨架（英文主体，中文字面保留）

```text
Create a highly detailed professional academic presentation slide in Simplified Chinese, red-gray color scheme on PURE WHITE background (#FFFFFF). Layout follows "Template A — top red banner":

================================
PAGE STRUCTURE (16:9, 3840×2160)
================================

1) Top red banner (full width, ~6% height), solid academic red (#C0392B), left-aligned white bold Chinese text, 60px left margin:
   "[成果章节大标题，例如：一、成果1: XXX 关键技术]"
   (思源黑体 Bold, 40pt, white)

2) Second-level title bar (full width, ~8% height): white band with a thick red (#C0392B) left vertical accent bar (12px). Contains on one row:
   Main line (Bold, 36pt, #2C3E50): "[主标题 X.X 一级节]"
   Sub line (Bold, 28pt, #C0392B): "[副标题 X.X.X 二级节]"

3) Abstract paragraph block (full width, ~10% height), 20pt black, justified, line-height 1.6. Exactly ONE red-highlighted bold clause (#C0392B) inside:
   "[摘要段落正文，其中一个关键短句用 **...** 标记为红色粗体]"

4) Three equal-width vertical panels (≈32% width each, 15px gutter, remaining ≈70% height). Each panel:
   • white background, 1px #BDC3C7 border, 8px rounded corners, subtle shadow (y=2, blur=6, 8% opacity)
   • red CHIP HEADER (#C0392B, 60px height, top rounded 8px), white-on-dark-red circled number ①②③, bold white Chinese title 24pt

================================
PANEL ① — [面板1主题]
Chip header: "① [面板1标题]"
================================
[面板1正文布局与内容：可以是机器人带引出线、三沙硬件对比、流程图等；
 若涉及多标注去重，请注入 fragments/anti_duplication_block.md 的强约束片段]

================================
PANEL ② — [面板2主题]
Chip header: "② [面板2标题]"
================================
[面板2内容]

================================
PANEL ③ — [面板3主题]
Chip header: "③ [面板3标题]"
================================
[面板3内容]

================================
FOOTER
================================
NO footer row. NO citations, page numbers, author names, journal references. Leave a clean 40px bottom white margin.

================================
GLOBAL DESIGN SPECIFICATIONS
================================
[复制粘贴 design_specifications.md 的关键约束]

Deliverable: one single 16:9 white-background slide faithfully reproducing Template A (top red banner + red-left-bar title row + justified abstract with one red clause + three ①②③ chip-header panels + NO footer).
```

## 常见变体

- **变体 A1 — 二面板 / 四面板版**：保留横幅与摘要段，把 `② / ③` 改为所需数量。但**强烈建议三面板**，视觉最稳。
- **变体 A2 — 摘要段 2–3 个红色高亮短语**：用于"三类传感器"这种并列结构。每个高亮短语用 `**...**` 标出并限定 2–5 字。
- **变体 A3 — 底部带核心贡献条**：在三面板下方追加一整行 `#FDEDEC` 浅红填充的 14–16pt 粗体条，替代关键科学问题。

## 使用注意

- Template A 的 header 是**圆角芯片**，不要用 Template B 的平底矩形红条；两者不可混用。
- 顶部红色横幅不要省略；若用户不需要成果编号，可改为"专项名称 / 课题名称"字样。
- 摘要段的红色高亮**只能在一个短句内部**，不要把整段文字染红。
