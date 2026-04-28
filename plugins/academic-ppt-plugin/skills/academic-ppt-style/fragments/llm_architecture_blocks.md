# Fragment — LLM 底层架构可视化块

本 fragment 收录 **大语言模型架构层面** 的高频视觉组件：Transformer、Attention、位置编码、归一化、MoE、KV Cache、推理优化等。全部遵循红灰学术风（`#C0392B` 主红 / `#2C3E50` 深灰 / 纯白背景 / 思源黑体）。

> 使用方法：在 Template A / B / C 的任一模块中，需要可视化某个架构组件时，**原样嵌入** 对应小节的英文描述。

---

## 1. Transformer Block（标准解码器）

```text
One vertically-stacked Transformer decoder block diagram, clean line-art style:
• Outer rounded rectangle labeled "Transformer Block × N" (22pt bold, dark gray)
• Inside, bottom-to-top:
  1) Input embedding arrow from below, labeled "x ∈ ℝ^{B×L×d}"
  2) RMSNorm / LayerNorm rounded box (#FDEDEC fill, red 1px border), label "Pre-Norm (RMSNorm)"
  3) Multi-Head Self-Attention block (wider box), label "MHA · h heads · causal mask"
     • Inside, show three parallel small chips: "Q = xWq", "K = xWk", "V = xWv"
     • Softmax(QKᵀ/√d_k)·V formula inline at the bottom
  4) Residual "+" node (small red circle with white plus sign)
  5) Second RMSNorm box
  6) Feed-Forward block, label "FFN (SwiGLU) · 4d hidden"
     • Inline formula: "FFN(x) = (Swish(xW₁) ⊙ xW₃) W₂"
  7) Second Residual "+" node
  8) Output arrow upward, labeled "→ next block"
• Connections: thin solid red arrows (2px), triangular heads
• Overall color: white body, red accents only on arrows / bullets / key labels
```

## 2. Multi-Head Attention 机制图

```text
Horizontal diagram of scaled dot-product attention with multi-head split:
• Left: input tensor "X ∈ ℝ^{B×L×d}" as a stacked-slab icon
• Three parallel projection arrows to Q / K / V, each labeled "Wq / Wk / Wv ∈ ℝ^{d×d}"
• Split Q/K/V into h heads via dashed vertical divider — show 4 slabs per tensor (stylized, representing h heads)
• For each head in parallel:
    Q · Kᵀ → scale by 1/√d_k → softmax → · V → head_i
• Concat(head_1, ..., head_h) box
• Final projection Wo → output "Y ∈ ℝ^{B×L×d}"
• Optional causal mask overlay: lower-triangular black matrix icon with label "Causal Mask"
• Use red for arrows and highlighted ops; gray #BDC3C7 for tensor boxes
• Inline formula below the diagram:
    "Attention(Q,K,V) = softmax(QKᵀ / √d_k) · V"
    "MHA(X) = Concat(head_1, …, head_h) · Wo"
```

## 3. Decoder-Only vs Encoder-Decoder vs Encoder-Only 三类对比

```text
Three side-by-side small schematics, each ~300px wide:
• (A) Encoder-Only (BERT-like):
    Input tokens [CLS] x1 x2 ... [SEP] → stacked Transformer encoder blocks → bidirectional attention (full-square mask icon) → contextual embeddings output
    Label: "Encoder-Only · BERT / RoBERTa · 双向注意力"
• (B) Encoder-Decoder (T5-like):
    Left column: source tokens → encoder blocks (bidirectional mask icon)
    Right column: target tokens → decoder blocks with causal mask + cross-attention arrows from encoder outputs
    Label: "Encoder-Decoder · T5 / BART · 跨注意力"
• (C) Decoder-Only (GPT-like):
    Input tokens → stacked Transformer decoder blocks → causal (lower-triangular) mask icon → next-token prediction head
    Label: "Decoder-Only · GPT / LLaMA · 自回归"
• Each schematic has a small example task tag at bottom: "分类 / 翻译 / 生成"
```

## 4. 位置编码对比（RoPE / ALiBi / Absolute）

```text
Three small comparison panels horizontally:
• Panel 1 — Absolute Sinusoidal (Vaswani 2017):
    Small heatmap of position embedding matrix (L × d), rainbow pattern
    Inline formula: "PE(pos, 2i) = sin(pos / 10000^{2i/d})"
    Note: "固定长度 / 外推能力差"
• Panel 2 — RoPE (Rotary Position Embedding):
    Small 2D rotation diagram: pair of vectors (q_{2i}, q_{2i+1}) being rotated by angle θ_i = pos · 10000^{-2i/d}
    Inline formula: "RoPE:  q_m' = R_Θ,m · q_m"
    Note: "相对位置 / LLaMA / 可外推"
• Panel 3 — ALiBi (Linear Bias):
    Small attention-score matrix with linearly-decreasing red gradient from diagonal outward
    Inline formula: "score_{i,j} = qᵢ·kⱼ − m · |i − j|"
    Note: "无参数 / 长度外推友好 / BLOOM"
• All three panels have uniform style: 1px #BDC3C7 border, white fill, 16pt bold title in red #C0392B
```

## 5. Pre-Norm vs Post-Norm

```text
Two tiny side-by-side diagrams:
• Left — Post-Norm (原版 Transformer):
    x → [Sublayer] → [LayerNorm] → [+ residual from x] → output
    Label: "Post-Norm · 训练不稳定 / 需 warmup"
• Right — Pre-Norm (GPT / LLaMA):
    x → [LayerNorm] → [Sublayer] → [+ residual from x] → output
    Label: "Pre-Norm · 训练稳定 / 深层可行"
• Use dashed gray arrows for residuals, solid red arrows for forward flow
```

## 6. MoE 混合专家模块

```text
Mixture-of-Experts layer diagram:
• Top: input token x → Router / Gating network (small rectangular box labeled "Router Gθ(x)")
• Router outputs top-k softmax weights over N experts (e.g., top-2 of 8)
• N expert branches shown as parallel small FFN boxes (8 boxes arranged in 2×4 grid), with the top-2 selected experts highlighted in red borders, the rest dimmed gray
• Selected experts' outputs weighted-sum → final output y
• Inline formula box (light red fill):
    "y = Σ_{i∈TopK} G(x)_i · E_i(x)"
    "Load-Balance Loss:  L_b = N · Σ f_i · P_i"
• Below the diagram, small specs strip:
    "专家总数 N = 8 · 激活专家 k = 2 · 激活参数 / 总参数 ≈ 25%"
• Include 2-3 bullet points on the side: "条件计算 · 容量提升 · 通信开销"
```

## 7. KV Cache 推理加速

```text
Two stacked timelines comparing inference with/without KV Cache:
• Upper timeline — "Without KV Cache":
    At each step t, recompute K, V for ALL past tokens [1..t]
    Time per token grows ~O(t²) total across T steps
    Visualize: rectangles with lengths increasing 1, 2, 3, ..., T (red)
• Lower timeline — "With KV Cache":
    At each step t, reuse cached K_{<t}, V_{<t} and only compute new k_t, v_t
    Time per token O(t) instead of O(t²) cumulative
    Visualize: uniform small rectangles of constant length (green), with a growing gray "cache" bar below
• Right side: cache diagram — a long gray strip representing KV memory, with red triangle pointers for "written this step" and gray for "reused"
• Bottom spec strip: "加速 ~Tx · 显存代价 O(B·L·2·n_layer·n_head·d_head)"
```

## 8. Flash Attention / I/O 感知优化

```text
Two-pane comparison:
• Left pane — "Standard Attention":
    Large attention matrix (L×L) written to HBM → read back for softmax → written again → read for V multiply
    Multiple red double-headed arrows between SRAM and HBM, labeled "HBM ↔ SRAM ·  O(L²) memory"
• Right pane — "FlashAttention (tiling + recomputation)":
    Tiled block-wise computation inside SRAM; softmax online-update; minimal HBM traffic
    Fewer arrows, short arrows inside SRAM block; HBM footprint O(L) only
• Small icon legend: HBM = larger gray slab, SRAM = smaller red slab, arrows = memory traffic
• Inline note: "FlashAttention v2 / v3 · IO-aware · ~2–3× speedup"
```

## 9. Tokenizer 与 BPE / SentencePiece 流程

```text
Horizontal pipeline:
• Input text box (e.g., "量子纠缠现象")
• → BPE / SentencePiece tokenizer box (red header)
• → Token ID sequence shown as colored chips: ["量", "子", "▁纠", "缠", "现", "象"]
• → Embedding table (stylized grid with a highlighted row for each token)
• → Output token embeddings x ∈ ℝ^{L×d}
• Below, small specs: "词表大小 |V| ≈ 128k · Byte-level fallback · 压缩比 ~2.8 char/token"
```

## 10. LLM 推理采样策略对比

```text
Four side-by-side small bar charts of next-token probability distributions, each applying a different sampling method:
• (a) Greedy (argmax) — single red bar at the top token
• (b) Top-k (k=50) — keep top 50 bars, renormalize (show a cutoff line)
• (c) Top-p / Nucleus (p=0.9) — keep smallest set covering cumulative 0.9 (shaded area)
• (d) Temperature T — show two overlays: T=0.7 (sharper) vs T=1.3 (flatter)
• Below each chart: a one-line description
• Use gray bars for candidates, red for selected mass
```

## 11. Scaling Law 曲线（Chinchilla 风格）

```text
Log-log line chart, 3 overlaid curves:
• X-axis: Training compute C (FLOPs), log scale, from 10^{19} to 10^{24}
• Y-axis: Test loss L, log scale
• Three power-law curves with shaded uncertainty bands:
    – Red curve — Chinchilla-optimal (slope ~ -0.34)
    – Blue curve — Kaplan 2020 (GPT-3 style, steeper)
    – Gray dashed — irreducible loss floor
• Annotations:
    "L(C) = L_∞ + (C_c / C)^α"
    "Chinchilla 最优: N_params ∝ C^{0.5},  D_tokens ∝ C^{0.5}"
• Put a single red-dot marker on each curve showing a reference model (e.g., "LLaMA-2-70B")
```

## 12. 参数高效微调（LoRA / QLoRA）

```text
LoRA adapter diagram:
• A frozen weight matrix W₀ ∈ ℝ^{d×d} shown as a gray large slab with "❄ Frozen" label
• In parallel, two small red slabs A ∈ ℝ^{d×r} and B ∈ ℝ^{r×d} (r ≪ d), labeled "Trainable, rank r"
• Forward flow: x → both paths → summed output y = W₀x + BAx
• Inline formula: "ΔW = BA,  rank(ΔW) ≤ r"
• Small specs chip: "典型 r = 8~64 · 可训参数量 ~0.1%–1%"
• Optional extension note below: "QLoRA = LoRA on top of 4-bit NF4 quantized W₀"
```

## 13. 量化（INT4 / NF4 / FP8）对比

```text
Three horizontal rows comparing weight storage formats:
• Row 1 — FP16: float16 slab of size "2 bytes/param"
• Row 2 — INT8: int8 slab + small scale/zero-point chip, "1 byte/param"
• Row 3 — NF4 (QLoRA): 4-bit NormalFloat slab + block-wise scale, "0.5 byte/param"
• Right side: memory footprint bar chart (horizontal) showing 70B-model memory:
    FP16 = 140 GB (longest red bar)
    INT8 = 70 GB
    NF4 = 35 GB (shortest, green)
• Bottom note: "量化带来的 perplexity 损失通常 < 1%（W4A16 / NF4）"
```

---

## 组合建议

- **"LLM 基础架构"页** → #1 Transformer Block + #2 Multi-Head Attention + #3 三类对比
- **"位置 / 归一化 / 采样选型"页** → #4 + #5 + #10
- **"推理效率优化"页** → #7 KV Cache + #8 FlashAttention + #13 Quantization
- **"Scaling & 微调"页** → #11 Scaling Law + #12 LoRA + #13 Quantization
- **"MoE 稀疏化"页** → #6 MoE + #11 Scaling Law（可选）+ 参考 `common_visual_blocks.md` 的 Ablation 表
