# 迭代清单与常见问题

当用户对上一版生成结果不满意时，按本清单**定位问题 → 定位 fragment → 精准修订**，避免重写整段 prompt。

---

## 一、常见问题速查表

| 问题症状 | 定位 | 推荐修订动作 |
|---|---|---|
| 某个引出线标签被画了两遍 | `fragments/anti_duplication_block.md` | 在该关键词处追加 "ONLY ONCE" 条款，并写入 FINAL VERIFICATION |
| 某个硬件被画成通用图标 / 编造外观 | `fragments/sensor_hardware_renderings.md` | 原样嵌入对应节的 realistic rendering 描述 |
| 出现了文献行 / 页码 / 作者名 | `SKILL.md` 铁律 5 | 在 FOOTER 段显式写 "NO footer. NO citations. NO page number." 并重申 |
| 顶部出现多余的红色横幅 / 版式串了 | `templates/template_*.md` 头部说明 | 明确指定 Template A / B / C 并声明 "NOT chip pills" 或 "NO top banner" |
| 底部的"关键科学问题"句被漏掉 | `template_B_top_left.md` D 段 | 把 D 段标注为 MANDATORY，并加一句 "centered, 26pt, bold, red #C0392B" |
| 摘要段被整段染红 | `SKILL.md` 设计铁律 8 | 在 Intro 段写 "ONLY 1–3 short keywords in red, NOT the whole sentence" |
| 模块内排版不对（应左文右图变成上下） | `templates/template_C_adaptive.md` Pattern 选择 | 在 CHOSEN PATTERN 处显式写出 C-4 并给出比例，如 "LEFT 35% / RIGHT 65%" |
| 版面太空 / 太挤 | `design_specifications.md` §1 | 调整 outer margin 与 gutter；把模块数从 3 改为 2 或 4 |
| 字体变成英文 / 非思源黑体 | `design_specifications.md` §3 | 重申 "思源黑体 (Source Han Sans SC) only; all Chinese must remain in simplified Chinese" |
| 颜色跑出红灰配色（出现粉 / 紫 / 渐变） | `design_specifications.md` §2 | 重申配色白名单并显式禁止 "No pink / purple / gradient / neon" |
| 机器狗姿态 / 朝向错 | `fragments/sensor_hardware_renderings.md` §1 | 显式指定 "head faces LEFT, tail faces RIGHT, 3/4 side-perspective" |
| 点云被画成几何点阵 / 无透视 | `fragments/sensor_hardware_renderings.md` §9 | 重申 "soft perspective, warm-to-cool gradient, horseshoe profile" |
| 时序图被画成柱状图 / 折线 | `fragments/common_visual_blocks.md` §1 | 重申 "horizontal timeline with parallel tracks, pulse markers, vertical red dashed line" |

---

## 二、每轮迭代的标准话术

在每一轮修订回复的**开头**写一句中文总结，例如：

> 本次关键改动：
> - 模块①的"边缘计算单元"与"顶部传感器扩展坞"已加入 ONLY ONCE 黑名单；
> - 摘要段中三个硬件名改为 inline 粗体红色，不再整段染红；
> - 底部"关键科学问题"改为版本 B（20 字凝练版）。

修订本体**只替换对应 fragment**，不要重写整页 prompt。

---

## 三、常见用户请求类型 → 应对策略

### 3.1 "把这段话凝练一下"

默认给三个长度的选项（A / B / C，长→中→短），让用户挑。示例长度：

- 正文摘要：~90 字 / ~50 字 / ~30 字
- 关键科学问题：~30 字 / ~20 字 / ~15 字
- 模块标题：≤ 16 字，无需给多版

### 3.2 "换个风格 / 照这张图做一张"

1. 先读参考图，判断属于 A / B / C 哪一类；
2. **向用户口头确认**："我判断这是 Template B / C-N，对吗？"；
3. 确认后再生成；若参考图无法归入 A / B / C 任一，**倾向用 C + 显式声明 pattern**。

### 3.3 "生成结果里有 X 问题"

按"问题速查表"定位对应 fragment，局部修订。**禁止默认做整页重写**。

### 3.4 "再加一个模块 / 少一个模块"

Template A / B 的栏数可在 2–4 之间调整（见 `templates/template_C_adaptive.md` pattern C-1）。超出此范围请改用其他 pattern。

### 3.5 "换硬件型号"

打开 `fragments/sensor_hardware_renderings.md`，若有对应节直接引用；若没有，**复制最接近的一节作为模板**，替换外形描述并保留格式。

---

## 四、"模型复读"问题的强化手段（按强度递增）

若 `anti_duplication_block.md` 的标准约束仍然压不住复读：

1. **提前重申**：在 prompt 最顶部加一句 `Draw exactly N labels. N. Not N+1. Not N-1.`
2. **引号隔离**：把易复读的关键词用引号包裹，如 `"Payload Bay"`，减少模型"创造性联想"
3. **分模块生成**：把该模块**单独**生成为一张图，再在 PPT / Figma 里手动拼版
4. **换前缀**：把 `(1)(2)..(7)` 换成 `①②...⑦`，有些模型对中文圆圈数字 follow 更稳
5. **人工兜底**：生成后 30 秒内手动删多余标签，是最稳的终极方案

---

## 五、Template 混用风险与规避

| 风险 | 场景 | 规避 |
|---|---|---|
| A 的圆角芯片 + B 的实心红条**混用** | 用户给了 B 参考图但 prompt 里残留 A 骨架 | 明确写 "module headers are SOLID RED RECTANGULAR BARS, NOT chip pills" |
| B 省略"关键科学问题"后**变成 A 但没有横幅** | 页面看起来"上不上下不下" | 省略"关键科学问题"时请改用 C + 显式 pattern，或加回横幅变回 A |
| C 的 pattern 未显式声明 | 模型自由发挥导致版式不稳 | 在 "CHOSEN PATTERN" 处必写一行 `C-N`，并写 "REASON FOR CHOICE" |

---

## 六、自检 checklist（生成提示词前）

在把 prompt 交给用户前，过一遍以下清单：

- [ ] 背景是 `#FFFFFF` 纯白（无彩色底 / 渐变）
- [ ] 主色只含 `#C0392B / #2C3E50 / #7F8C8D / #BDC3C7 / #FDEDEC / #ECF0F1`
- [ ] 字体显式声明 "思源黑体 / Source Han Sans SC"
- [ ] 分辨率声明 "3840×2160 (4K), 16:9"
- [ ] FOOTER 段显式写 "NO footer / NO citations / NO page number"
- [ ] 所有硬件都有对应的 realistic rendering 描述（不是 icon）
- [ ] 所有多标注视觉都注入了 anti_duplication_block
- [ ] 模板骨架层（标题 / 摘要 / 主体 / 收束）四层齐全
- [ ] 若使用 Template C，CHOSEN PATTERN 已显式声明
- [ ] 摘要段只在关键短语 / 单句内加红色高亮，非整段
