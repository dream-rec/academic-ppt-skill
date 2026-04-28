# Fragment — 视觉语言模型与多模态可视化块

本 fragment 覆盖 **VLM / 多模态大模型** 相关视觉组件：CLIP 对比学习、LLaVA 架构、Flamingo 跨注意、图像 token 化、VLA、视频理解、Any-to-Any 等。

---

## 1. CLIP 对比学习架构

```text
Symmetric two-tower contrastive learning diagram:
• Left tower — Image Encoder (ViT / ResNet):
    Image input → Vision Transformer blocks → pooled image embedding e_v ∈ ℝ^d
• Right tower — Text Encoder (Transformer):
    Text input → Transformer blocks → pooled text embedding e_t ∈ ℝ^d
• Both towers output L2-normalized embeddings that meet at a central similarity matrix:
• Central matrix S ∈ ℝ^{N×N} visualized as a checkerboard:
    – Diagonal cells highlighted in red (positive pairs)
    – Off-diagonal cells in light gray (negative pairs)
• InfoNCE loss formula box (light red fill):
    "L = -½ (Σ_i log(exp(s_{ii}/τ) / Σ_j exp(s_{ij}/τ)) + Σ_i log(exp(s_{ii}/τ) / Σ_j exp(s_{ji}/τ)))"
• Small specs strip: "温度系数 τ · 400M image-text pairs · Batch 32k"
• Red solid arrows for forward flow; dashed red lines for positive-pair alignment
```

## 2. LLaVA 风格 VLM 架构

```text
Horizontal three-stage architecture:
• Left block — Vision Encoder (frozen):
    Image → CLIP-ViT-L/14 → grid of visual tokens z_v ∈ ℝ^{N_v × d_v}
    Snowflake icon ❄ indicating frozen weights
• Middle block — Projector (trainable):
    Small MLP / Q-Former box, maps d_v → d_llm
    Fire icon 🔥 indicating trainable
    Inline note: "2-layer MLP projector (LLaVA-1.5) 或 Q-Former (BLIP-2)"
• Right block — LLM (partially trainable):
    Concatenated sequence [visual tokens] + [text tokens] → LLM (e.g., Vicuna / LLaMA)
    Output: autoregressive text response
    Snowflake or fire icon depending on stage
• Bottom annotation: "Training Stage 1: align projector only · Stage 2: instruction tune LLM + projector"
• Small callout on a token chip: "每张图 ≈ 576 个 visual tokens (14×14 patch grid + CLS)"
```

## 3. Flamingo / Cross-Attention 插入式架构

```text
Frozen LLM backbone (vertical stack of decoder blocks) with inserted cross-attention blocks:
• LLM blocks shown as tall gray stacked rectangles (frozen, ❄ icon)
• Between every 2–4 LLM blocks, insert a red cross-attention block:
    – Queries from LLM hidden state
    – Keys / Values from Perceiver Resampler output of visual features
• Left side: video / image frames → Vision Encoder → Perceiver Resampler → fixed-length visual tokens
• Bidirectional dashed arrows from visual tokens into each inserted cross-attention block
• Top of LLM outputs text autoregressively
• Labels: "Frozen LM · Inserted Gated Cross-Attention · Few-Shot Multimodal"
• Small note: "Flamingo / IDEFICS · 支持交错图文输入"
```

## 4. 图像 Token 化对比

```text
Three-panel comparison of visual tokenization schemes:
• Panel A — Patch Embedding (ViT style):
    Image split into 14×14 regular grid of patches → flatten → linear proj → token sequence
    Label: "固定网格 · 简单 · 信息冗余"
• Panel B — Q-Former (BLIP-2):
    N learnable queries (e.g., 32) attend over full image features → compressed to N tokens
    Label: "查询式压缩 · 参数高效"
• Panel C — Native Resolution / Any-Resolution:
    Image tiled into variable-count patches preserving aspect ratio; position interpolated
    Label: "可变分辨率 · Qwen-VL / InternVL 2.x"
• Small visualization per panel: stylized image + token sequence of different lengths
```

## 5. 多模态融合位置对比（Early / Late / Hybrid）

```text
Three side-by-side micro-diagrams:
• Early Fusion: raw image features + raw text tokens concatenated → single transformer
• Late Fusion: separate image tower + text tower → late similarity / concat head
• Hybrid / Cross-Attn: both towers run in parallel with cross-attention between them
• Each micro-diagram has 2 small colored blobs (image-red, text-blue) and arrows/connections indicating the fusion type
• Bottom note: "主流 VLM 采用 Early Fusion (LLaVA) 或 Hybrid (Flamingo/Chameleon)"
```

## 6. 多模态对齐训练阶段

```text
Horizontal timeline with 4 stages connected by red arrows:
• Stage 1 — 模态对齐预训练 (Vision-Language Alignment):
    大规模图文对 (LAION, COYO) → 训练 projector
    Data: ~600M image-text pairs
• Stage 2 — 视觉指令微调 (Visual Instruction Tuning):
    合成 / 人工 QA 指令数据 (LLaVA-Instruct 150k)
    训练: projector + LLM (LoRA)
• Stage 3 — 领域适配 (Domain Tuning):
    OCR / Chart / Document / Math 专项数据
    多样化任务指令
• Stage 4 — 偏好对齐 (Visual DPO / RLHF-V):
    基于幻觉 / 事实性偏好对进行 DPO
    降低 hallucination
• Each stage has small icon + 2 bullet points + data volume chip
```

## 7. Visual Chain-of-Thought / 推理可视化

```text
Vertical reasoning trace over an input image:
• Top: input image thumbnail with bounding boxes and arrows highlighting key regions
• Middle: step-by-step chain-of-thought text blocks, each prefixed with red "Step k:" label
    Step 1: "观察到左上角有..."
    Step 2: "结合题目条件..."
    Step 3: "计算可得..."
• Each CoT step may reference a specific bounding box via a small red dashed line to the image
• Final answer box at the bottom, light red fill, bold: "Answer: ..."
• Side note: "Set-of-Mark (SoM) · Visual CoT · GPT-4V / Qwen2-VL"
```

## 8. 幻觉 (Hallucination) 对比示意

```text
Side-by-side two-column comparison:
• Left column — "不当行为 · Hallucination":
    Image thumbnail + model output with red-underlined hallucinated phrases
    e.g., "图中有一只<狗>" (image actually shows no dog)
    Root causes (bullet list): "对象共现偏置 · 过拟合语言先验 · 视觉 token 信息损失"
• Right column — "缓解策略":
    • Visual DPO (preference data on factual vs hallucinated)
    • CHAIR / POPE 评测循环
    • Segmentation-grounded responses
    • CFG / Contrastive decoding
• Bottom: small KPI chip "CHAIR_s ↓ 30% · POPE F1 ↑ 5%"
```

## 9. VLA (Vision-Language-Action) 具身基础模型

```text
End-to-end VLA pipeline, horizontal:
• Left: observation stack (RGB image + optionally depth / language instruction)
• → Vision encoder (ViT / CLIP)
• → Language encoder (or unified LLM)
• → Action decoder head
• Right: action tokens a_t (e.g., 7-DoF gripper pose + gripper open/close), discretized or continuous
• Include small side callout:
    "Action 表示:  离散 token (RT-2) · 连续回归 (OpenVLA) · Diffusion (π₀ / Octo)"
• Below: tiny inline note — "训练: 大规模机器人 demonstration (Open X-Embodiment)"
```

## 10. Video LLM / 长视频理解

```text
Horizontal timeline of a long video:
• Top: filmstrip of N frames (small thumbnails)
• Sampling strategies shown as differently-colored markers on the filmstrip:
    Uniform sampling (red dots at equal intervals)
    Keyframe sampling (gray dots at scene cuts)
    Query-adaptive sampling (green dots concentrated on relevant moments)
• Middle: temporal compression via token merging / Q-Former / memory bank
• Right: LLM receives compressed token sequence → text answer to query
• Spec chip: "输入时长 ~ 1 小时 · Token 压缩比 ~ 100×"
• Side note: "LongVA / Video-LLaMA / VILA"
```

## 11. Any-to-Any 多模态（统一生成模型）

```text
Unified any-to-any model wheel diagram:
• Central circle labeled "Unified Transformer" (red fill with white bold text)
• Around the circle, 6–8 input/output modality icons arranged radially:
    Text · Image · Video · Audio · Speech · 3D · Action
• Bidirectional arrows between each modality and the center (solid red for supported, dashed gray for partial)
• Small badges beneath select modalities: "Chameleon · GPT-4o · Gemini · EMU3"
• Inline note: "统一 tokenization → 统一 autoregressive objective"
```

## 12. 多模态评测体系

```text
Multi-dimensional radar chart (hexagonal):
• 6 axes labeled: "感知 Perception · 推理 Reasoning · OCR · 数学 Math · 幻觉 Hallucination ↓ · 指令遵循"
• Two polygons overlaid: one red (proposed model), one gray (baseline)
• Axis scales 0–100; values shown as small numeric labels at the vertices
• Legend at bottom-right: Red = Ours, Gray = Baseline
• Accompanying mini table (right side) listing 4–6 benchmarks per axis:
    Perception: MMBench, SEED-Bench
    Reasoning: MMMU, MathVista
    OCR: DocVQA, TextVQA
    Math: MathVerse
    Hallucination: POPE, HallusionBench
    Instruction: MIA-Bench
```

---

## 组合建议

- **"VLM 架构综述"页** → #2 LLaVA + #3 Flamingo + #5 融合方式对比
- **"视觉表征选型"页** → #1 CLIP + #4 图像 tokenization 对比
- **"训练流程"页** → #6 四阶段对齐 + 引用 `training_pipeline_blocks.md` §4 DPO
- **"幻觉与安全"页** → #7 Visual CoT + #8 Hallucination 对比
- **"具身 / VLA"页** → #9 VLA + `embodied_ai_blocks.md` 的 world model 块
- **"长视频 & 统一生成"页** → #10 Video LLM + #11 Any-to-Any
- **"评测成果"页** → #12 雷达图 + `evaluation_and_results_blocks.md` leaderboard
