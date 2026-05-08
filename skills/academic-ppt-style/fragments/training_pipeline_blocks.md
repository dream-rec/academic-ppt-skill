# Fragment — 训练流程可视化块（Pretrain / Post-train）

本 fragment 覆盖 LLM 从**预训练 → SFT → RLHF / DPO / GRPO → Alignment**的完整训练流水线相关视觉组件。

---

## 1. 预训练数据处理流水线

```text
Horizontal 6-stage pipeline with red arrows between stages:
• Stage 1 — 原始语料采集 (Crawl / Archive / Github / Books)
    Icons: globe, code, book; raw bytes indicator
• Stage 2 — 语种识别与过滤 (Language ID, FastText)
    Note: "保留目标语种 · 过滤二进制"
• Stage 3 — 质量评分 (Perplexity / KenLM / classifier)
    Note: "低质量文档剔除"
• Stage 4 — 去重 (MinHash / SimHash / exact dedup)
    Note: "文档级 + 段落级去重"
• Stage 5 — 有害内容过滤 (Toxicity / PII scrubbing)
    Note: "去除敏感 · 合规"
• Stage 6 — 打包与采样 (Tokenize → pack → shuffle)
    Note: "按配比混合 · Streaming"
• Below the pipeline, a data-volume bar chart showing attrition:
    Raw 45T → Cleaned 8T → Tokens 2T (bars in decreasing length, red)
• Small note: "典型清洗保留率 ~15–20%"
```

## 2. 预训练数据配比饼图

```text
Donut chart with 6–8 colored sectors (red-gray-accent palette):
• Web crawl (CommonCrawl / refined) — largest sector, e.g., 55%
• Code (GitHub / StackExchange) — 15%
• Books / Papers — 10%
• Wikipedia / reference — 5%
• Math / reasoning — 5%
• Multilingual — 5%
• Dialogue / forums — 3%
• Other — 2%
• Center label: "Pretraining Corpus · Total ≈ 15T tokens"
• Legend on the right with percentages and a small sample-source line per category
```

## 3. SFT → RLHF 三阶段对齐流程

```text
Three vertically-stacked training stages, each as a rounded rectangle with red header:
• Stage I — Supervised Fine-Tuning (SFT):
    Diagram: (prompt, response) pairs → frozen LM → next-token loss → LM'
    Formula: "L_SFT = -Σ log p_θ(y_t | x, y_{<t})"
    Data note: "高质量人工指令数据 10k–100k 条"
• Red downward arrow labeled "→ 输出参考策略 π_SFT"
• Stage II — Reward Model Training:
    Diagram: prompt x → π_SFT produces (y_w, y_l) pair → human ranks preference → reward model r_φ trained with Bradley-Terry loss
    Formula: "L_RM = -E[log σ(r_φ(x, y_w) − r_φ(x, y_l))]"
    Data note: "偏好对 50k–200k 条"
• Red downward arrow labeled "→ 输出奖励模型 r_φ"
• Stage III — RLHF (PPO):
    Diagram: policy π_θ (init from SFT) rollouts → reward r_φ + KL to SFT → PPO update
    Formula: "L_PPO = E[r_φ(x,y)] - β · KL(π_θ ‖ π_SFT)"
    Data note: "Online rollouts + KL 正则"
• Each stage box has a small icon strip on the right (data / model / loss)
```

## 4. DPO (Direct Preference Optimization) 对比流程

```text
Two side-by-side comparison panels:
• Left — "Classic RLHF (3-step)":
    SFT → Reward Model → PPO
    Icons for 3 models, 2 arrows
    Note: "需要奖励模型 · 在线采样 · 实现复杂"
• Right — "DPO (1-step)":
    SFT → directly optimize on preference pairs
    Single-step arrow; no reward model needed
    Formula box (#FDEDEC):
      "L_DPO(π_θ; π_ref) = -E_{(x, y_w, y_l)} [ log σ( β log (π_θ(y_w|x)/π_ref(y_w|x)) − β log (π_θ(y_l|x)/π_ref(y_l|x)) ) ]"
    Note: "隐式奖励 · 离线训练 · 简化 pipeline"
• Below both panels, a unified KPI strip:
    训练稳定性 · 算力开销 · 超参数敏感度 · 最终对齐效果
    (两列打分：RLHF vs DPO，用红 / 灰条对比)
```

## 5. PPO / GRPO 采样回路

```text
Circular / looped diagram of one policy-gradient iteration:
• Nodes (clockwise starting at 12 o'clock):
  1) Prompt buffer D_prompt
  2) Policy π_θ samples response y
  3) Environment / Reward model r_φ scores (x, y)
  4) Advantage estimation (GAE or group-relative in GRPO)
  5) KL penalty vs reference π_ref
  6) Policy update via PPO clipped objective
• Red solid arrows connect nodes; node 6 loops back to node 2
• Center label: "On-Policy RL Loop" and the clipped-objective formula:
    "L^CLIP = E[ min(r_t A_t, clip(r_t, 1-ε, 1+ε) A_t) ]"
• GRPO-specific callout (small box):
    "GRPO: 组内相对奖励, 省去 value model"
    "A_i = (r_i - mean(r)) / std(r)"
```

## 6. RLAIF / Constitutional AI 对比

```text
Two vertical lanes, side by side:
• Lane A — RLHF (Human Feedback):
    人类标注员 → 偏好数据 → 奖励模型 → PPO
    Icon: human silhouette cluster
    Label: "成本高 · 一致性难保证"
• Lane B — RLAIF (AI Feedback):
    另一个 LLM (critic) → 按宪法原则评分 → 偏好数据 → RL / DPO
    Icon: robot cluster
    Label: "可规模化 · 可审计 · Anthropic / Constitutional AI"
• Arrow from Lane A to Lane B labeled "↓ 替换标注主体"
• Below both lanes: shared downstream "Aligned Policy π_θ*"
```

## 7. 训练损失曲线（Loss Curve）

```text
Line chart with two curves on same axes:
• X-axis: training steps (0 → T), linear
• Y-axis: loss (log scale typical)
• Red solid curve — training loss (decreasing, slight jagged)
• Gray dashed curve — validation loss (decreasing, slightly above training)
• Optional shaded band: ±σ around validation curve
• Annotations:
  – Vertical dashed line at step S_wu: "warmup 结束"
  – Arrow pointing to inflection: "loss spike · grad clip 生效"
  – End-of-training marker with final loss value
• Grid: 1px light gray
• Chart frame: 1px #BDC3C7, title "训练损失曲线" top-left
```

## 8. Scaling Law / Compute-Optimal 参考

> 视觉块见 `llm_architecture_blocks.md` §11（避免重复）。此处补充一个**训练视角的变体**：

```text
Compute-optimal frontier scatter-plot:
• X-axis: Model params N (log), from 1B to 1T
• Y-axis: Training tokens D (log), from 100B to 10T
• Red dashed line: N · D = C (Chinchilla optimal, slope -1 in log-log)
• Scatter points for reference models:
  – GPT-3 175B @ 300B tokens (undertrained, above line)
  – LLaMA-2 70B @ 2T tokens (on or below line)
  – Chinchilla 70B @ 1.4T (near line)
  – DeepSeek-V3 671B (MoE, special marker)
• Each point has a small label
• Inset: "给定 compute C = 6 N D, 最优比 N : D ≈ 1 : 20"
```

## 9. 指令数据构造流水线

```text
Pipeline diagram for instruction dataset synthesis:
• Stage 1 — Seed Prompts: small set of human-curated prompts (~175 tasks)
• Stage 2 — Self-Instruct / Evol-Instruct: teacher LLM expands prompts
    Branch icons: "depth-evolve · breadth-evolve · reasoning-evolve"
• Stage 3 — Response Generation: teacher LLM produces responses
• Stage 4 — Quality Filtering: length, rouge-overlap, classifier, LLM-judge
• Stage 5 — Deduplication & Balance: similarity dedup, domain balancing
• Stage 6 — Final SFT / DPO dataset
• Data-volume annotations above stages: "175 → 52k → 70k → 52k → 38k → 38k"
• Red arrows connecting stages; small icon per stage
```

## 10. 训练基础设施拓扑

```text
Abstracted cluster topology diagram:
• Top: a row of N "Node" boxes (e.g., 8 nodes), each containing 8 small GPU chips in a 2×4 grid, labeled "H100 × 8"
• Intra-node: red bi-directional NVLink arrows between GPUs within a node
• Inter-node: thicker red arrows labeled "InfiniBand 400 Gbps" connecting nodes through a central switch
• Left side annotations: "Tensor Parallelism (TP) · 节点内"
• Right side annotations: "Pipeline Parallelism (PP) · 跨节点"
• Bottom annotations: "Data Parallelism (DP) + ZeRO-3 · 全局"
• Legend chip: "3D Parallelism: TP × PP × DP"
• Small performance strip: "GPU 有效利用率 MFU ~45–55% · 通信 / 计算重叠"
```

## 11. 评测对齐：Reward Hacking 可视化

```text
Two-curve chart illustrating reward hacking / overoptimization:
• X-axis: KL(π_θ ‖ π_ref) — policy divergence from reference
• Y-axis (dual):
  – Red solid curve: Proxy reward r_φ(x, y) — monotonically increasing
  – Gray solid curve: True human preference win-rate — inverted-U shape
• Vertical dashed red line at the peak of the gray curve, labeled "最优 KL 预算"
• Shaded region beyond the peak labeled "Reward Hacking Zone"
• Caption below: "过度优化代理奖励会偏离真实偏好 · 引入 KL 正则抑制"
```

## 12. Catastrophic Forgetting / Continual Learning

```text
Multi-panel chart:
• Left panel: accuracy on Task-1 over sequential training on Tasks 1→2→3
    Red curve drops at Task-2 boundary, drops further at Task-3 boundary
    Label: "连续微调下 Task-1 性能下降"
• Right panel: mitigation strategies as a horizontal chip row:
    "数据回放 · LoRA 隔离 · EWC 正则 · Mixture-of-Experts · Model Merging"
• Arrow between panels: "→ 缓解策略"
• Small note: "关键: 保持 π_ref 分布不漂移"
```

---

## 组合建议

- **"预训练系统"页** → #1 数据流水线 + #2 配比饼图 + #10 集群拓扑
- **"对齐全流程"页** → #3 三阶段 RLHF + #4 DPO 对比
- **"强化学习对齐"页** → #5 PPO/GRPO 回路 + #11 Reward Hacking
- **"数据为王"页** → #1 + #9 指令数据 + #2 饼图
- **"训练过程分析"页** → #7 损失曲线 + #11 Reward hacking + `evaluation_and_results_blocks.md` 的 Ablation
