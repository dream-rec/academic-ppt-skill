# Fragment — 多标注引出线的反复读强约束

## 用途

当模块内包含一个主视觉（如机器狗、系统架构图、传感器布置图）**带多条引出线标注**时，扩散类文生图模型（Gemini Image / GPT Image / Nano Banana 等）容易出现：

- 同一标签被画两遍
- 某些编号缺失
- 引出线交叉成一团乱麻
- 生成了空的白色标签框
- 标签文字被模型"创造性改写"成近义词

本 fragment 通过"固定清单 + 空间绑定 + 黑名单条款 + 终审自检"四层约束压制上述问题。

## 使用方法

在对应 Panel / Module 的 body 描述里**原样嵌入下面的代码块**，并把 `<ITEM_N>`、`<ANCHOR_N>`、`<POSITION_N>` 这些占位符替换为具体内容。

---

## 可复用模板（原样嵌入英文 prompt 中）

```text
===== STRICT CALLOUT CONSTRAINTS (READ AND OBEY) =====
You MUST place EXACTLY <N> numbered callouts around the main visual. The callout set is a FIXED CHECKLIST. Each item below is UNIQUE and must appear EXACTLY ONCE. No label may be drawn twice. No label may be split across two boxes. No synonym substitution is allowed. If you find yourself about to draw a label that was already placed, STOP and skip it. After drawing, count visible boxes — if count ≠ <N>, remove the extras.

The <N> callouts are strictly bound to anchor components AND to spatial positions on the canvas:

(1)  <ITEM_1 中文标注文本>
     → point to: <ANCHOR_1，例如："robot head front housing">
     → label position: <POSITION_1，例如："UPPER-LEFT of the main visual">
     → leader line goes from <ANCHOR_1> to the label

(2)  <ITEM_2>
     → point to: <ANCHOR_2>
     → label position: <POSITION_2>
     → leader line goes from <ANCHOR_2> to the label

... （重复到 (N)）

===== ADDITIONAL ANTI-DUPLICATION RULES =====
• The <N> label boxes form a set. No two boxes may contain identical or near-identical Chinese text.
• Each of the numbers (1)..(<N>) must appear exactly once, in the exact Chinese text above, with no paraphrasing.
• <KEY_TERM_A> belongs to callout (<idx_A>) ONLY. Do NOT create a second label with this term.
• <KEY_TERM_B> belongs to callout (<idx_B>) ONLY. Do NOT create a second label with this term.
  (为每一个历史上易复读的关键词都写一条 "ONLY ONCE" 条款)
• Do NOT draw any extra unlabeled boxes, empty boxes, placeholder boxes, or decorative duplicate boxes.
• Total number of white-with-red-border callout boxes MUST equal <N>. Not <N-1>, not <N+1>.

===== CALLOUT VISUAL STYLE =====
• Each callout: white fill, 1px #C0392B border, 4px corner radius, 13–14pt 思源黑体 dark-gray (#2C3E50) text, inner padding 8px.
• Leader line: 1px hairline #C0392B, starting with a 4px solid red dot at the component anchor, then a straight segment, optionally ending in a short horizontal elbow into the left edge of the label box.
• Leader lines MUST NOT cross each other. Route them so that they form a clean radial pattern around the main visual.
• The number prefix "(n)" sits inside each label box on the left, in red (#C0392B), followed by the Chinese text in dark gray.

===== FINAL VERIFICATION (MANDATORY) =====
Before outputting the panel, verify ALL of the following simultaneously:
  ☑ Exactly <N> numbered callout boxes are visible.
  ☑ Numbers (1) through (<N>) each appear exactly once, in order, no repetition, no gap.
  ☑ No two callout labels share the same or near-identical Chinese text.
  ☑ Every "ONLY ONCE" keyword above appears in exactly one callout box.
  ☑ No empty placeholder boxes.
  ☑ Leader lines do not cross; every leader line has both a source anchor and a destination label.
If any check fails, redraw the panel.
```

---

## 参数填充速查

| 占位符 | 示例 | 说明 |
|---|---|---|
| `<N>` | `7` | 引出线数量，**建议 5–9**；超过 9 会严重挤版 |
| `<ITEM_N>` | `头部双目视觉相机（ZED 2）` | 中文标注文本，保持原样不改写 |
| `<ANCHOR_N>` | `the small rectangular stereo camera mounted on the ROBOT HEAD` | 英文描述，指向主视觉的**具体部位** |
| `<POSITION_N>` | `UPPER-LEFT / MIDDLE-LEFT / LOWER-LEFT / UPPER-RIGHT / MIDDLE-RIGHT / LOWER-RIGHT / BOTTOM-CENTER / TOP-CENTER` | 标签在画布上的**方位** |
| `<KEY_TERM_X>` | `"Payload Bay"`, `"Jetson Orin"`, `"ZED 2"`, `"Livox Mid-360"` | 历史上易复读的关键词，每个都单独写一条 ONLY ONCE |

## 推荐的空间位置分布（按 N 取）

- **N = 5**：UPPER-LEFT, LOWER-LEFT, TOP-CENTER, UPPER-RIGHT, LOWER-RIGHT
- **N = 6**：UPPER-LEFT, MIDDLE-LEFT, LOWER-LEFT, UPPER-RIGHT, MIDDLE-RIGHT, LOWER-RIGHT
- **N = 7**：UPPER-LEFT, MIDDLE-LEFT, LOWER-LEFT, UPPER-RIGHT, MIDDLE-RIGHT, LOWER-RIGHT, BOTTOM-CENTER
- **N = 8**：在 7 的基础上加 TOP-CENTER
- **N = 9**：在 8 的基础上加 MIDDLE-CENTER-BELOW（穿主图下方）

## 额外建议（经验）

1. 若生成后仍有重复，把该模块**单独生成**（不与其他模块同图），重复率显著下降。
2. 生成后用 PPT / Figma / Keynote **手动删除多余标签** 是最稳的兜底，花 30 秒。
3. 对 `"(n)"` 这种数字前缀，必要时写成 `"〔n〕"` 或 `"①②③..."`，有些模型对西文圆括号识别更稳。
4. 在 prompt 开头再重复一次 `Draw exactly N labels. N. Not N+1.`，对"序列 follow"型模型有效。
