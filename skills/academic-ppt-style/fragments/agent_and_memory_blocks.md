# Fragment — Agent 架构与记忆系统可视化块

本 fragment 覆盖 **LLM Agent / 多智能体 / 记忆系统 / RAG / Tool Use** 相关视觉组件。

---

## 1. ReAct 循环 (Reason + Act)

```text
Circular loop diagram with 4 nodes:
• Node 1 (top) — "Thought 思考":
    LLM reasons about current state, plans next step
• Node 2 (right) — "Action 行动":
    LLM emits structured tool call (e.g., JSON function call)
• Node 3 (bottom) — "Observation 观察":
    Tool executes, returns result to LLM context
• Node 4 (left) — "Reflection 反思":
    LLM evaluates, decides continue / terminate
• Red solid arrows connecting nodes clockwise, forming a closed loop
• Center label: "ReAct Loop · N iterations"
• Side callout: typical prompt format "Thought: ... / Action: ... / Observation: ..."
• Termination condition chip: "Action = Final Answer 或 迭代上限"
```

## 2. Planner-Executor 双层架构

```text
Two-tier hierarchical diagram:
• Upper tier — Planner (strong reasoning LLM, e.g., GPT-4o / Claude):
    Receives high-level goal → decomposes into subtasks (tree or DAG)
    Icon: brain / map
• Downward arrow labeled "任务分解 · Plan"
• Lower tier — N Executor workers (specialized / cheaper LLMs):
    Each worker handles one subtask with concrete tool calls
    Icons: workers in a row
• Upward arrow labeled "结果汇总 · Aggregate"
• Optional monitor chip on the side: "Self-Reflection / Self-Consistency"
• Below: small note "Plan-and-Solve · LATS · Language Agent Tree Search"
```

## 3. 工具使用 (Tool Use) 调用流

```text
Horizontal swim-lane diagram:
• Lane 1 — User: sends query
• Lane 2 — LLM Agent: parses intent, selects tool from registry
    Tool Registry shown as a side panel with N tool chips (Search, Code, Calculator, DB, API, ...)
• Lane 3 — Selected Tool: executes, returns JSON result
• Lane 4 — LLM Agent: incorporates result, continues reasoning OR selects next tool
• Back to Lane 1: final answer
• Arrows colored red; each step labeled with a timestamp or step number
• Include inline note on tool schema:
    "Function Calling Schema: {name, description, parameters (JSON Schema)}"
```

## 4. Multi-Agent 协作拓扑

```text
Graph-based multi-agent diagram. Show 3 common topologies side-by-side:
• Topology A — Centralized (Orchestrator):
    One central "Manager" agent in the middle; N worker agents arranged in a ring connected to it
    Flow: Manager dispatches → workers execute → workers report back
• Topology B — Debate / Peer:
    3–4 agents in a ring, each with bidirectional arrows to the others
    Label: "Multi-Agent Debate · Self-Consistency via Disagreement"
• Topology C — Hierarchical:
    Tree with root manager, mid-level coordinators, leaf workers
    Label: "Hierarchical MAS · MetaGPT / ChatDev"
• Each agent node is a small rounded rectangle with a role label (e.g., "Researcher", "Critic", "Coder")
```

## 5. 记忆系统分层

```text
Three-tier memory hierarchy (inspired by cognitive science):
• Top tier — Working Memory (Context Window):
    Short rectangular slab labeled "当前会话 · 4k–1M tokens"
    Icon: clipboard
    Volatile
• Middle tier — Episodic Memory (短期记忆):
    Timeline strip of past interactions, recent events highlighted
    Icon: film strip
    Stored in vector DB with timestamps
• Bottom tier — Semantic Memory (长期记忆 · Knowledge):
    Large slab labeled "知识库 / 事实 / 用户画像"
    Icon: library
    Structured KB + dense vectors
• Red arrows between tiers: "写入 (write)" downward, "检索 (retrieve)" upward
• Side callout: "Memory decay · Forgetting mechanism · MemGPT / MemoryBank"
• Bottom spec: "Embedding Model (BGE / E5) + VectorDB (Chroma / Qdrant / Milvus)"
```

## 6. RAG (Retrieval-Augmented Generation) 流水线

```text
Two-phase pipeline:
• Offline Phase (top half):
    Documents → Chunking (512-1024 tokens) → Embedding (BGE / E5) → VectorDB (Qdrant / Milvus)
    Icons: file → scissors → vector strip → cylinder
• Online Phase (bottom half):
    User Query → Query Embedding → Top-k retrieval from VectorDB → Rerank (cross-encoder) → stuff into LLM context → answer
    Icons: person → vector → database → sort → LLM → speech bubble
• Red arrows connecting all stages
• Side note: "Chunk Size / Overlap / Hybrid BM25+Vector / Reranking"
• Bottom KPI chip: "Recall@k · MRR · Answer Faithfulness · Latency"
```

## 7. Advanced RAG 技术矩阵

```text
2×3 grid of advanced RAG techniques, each cell is a small card:
• (a) Query Rewriting: user Q → LLM rewrites → multiple sub-queries
• (b) HyDE: LLM hallucinates an ideal answer → embed → retrieve
• (c) Reranking: cross-encoder re-scores top-k results
• (d) Self-RAG: model decides if retrieval is needed, reflects on quality
• (e) Graph RAG: entities + relations, hop-based retrieval over KG
• (f) Agentic RAG: agent iteratively refines queries based on gaps
• Each card: icon + 2-line description + representative method name
• Red borders on cards, white fill
```

## 8. Function Calling vs ReAct vs Plan-Execute 对比

```text
Three-column comparison table (minimal style, not full table):
• Column A — Native Function Calling (OpenAI / Anthropic / Gemini):
    Single-turn tool use; model outputs structured JSON
    Pros: simple, reliable
    Cons: limited chaining
• Column B — ReAct Loop:
    Multi-turn Thought-Action-Observation; prompt-engineered
    Pros: flexible; any LLM
    Cons: parse errors, long traces
• Column C — Plan-Execute-Observe:
    Explicit plan tree; separate planner & executor
    Pros: long-horizon tasks
    Cons: plan-drift, expensive
• Each column has a small icon diagram at the top + bullet pros / cons below
```

## 9. Agent 评测框架

```text
Hexagonal radar chart comparing agents on 6 axes:
• Task completion rate
• Step efficiency (fewer is better)
• Cost (tokens / $)
• Robustness (error recovery)
• Safety (refusal of harmful actions)
• Generalization (unseen tools)
• Two overlaid polygons: red = our agent, gray = baseline
• Side table listing benchmarks per axis: "AgentBench · WebArena · ToolBench · GAIA · OSWorld · SWE-Bench"
```

## 10. Context Engineering / Prompt 模板栈

```text
Vertical stack showing the composition of a typical agent prompt:
• Layer 1 (top) — System Prompt (role, constraints, safety)
• Layer 2 — Tool Registry (function schemas)
• Layer 3 — Retrieved Memory (episodic events + semantic facts)
• Layer 4 — Few-Shot Exemplars (optional)
• Layer 5 — Current Conversation History
• Layer 6 (bottom) — User's Latest Message
• Each layer is a horizontal rounded rectangle with a small icon + title + token-count estimate
• Total at the bottom: "Total context: ~12k tokens"
• Arrow on the right side labeled "→ LLM" exiting the stack
```

## 11. Agent Memory 管理策略

```text
Horizontal 3-block diagram of memory write policies:
• Block A — Sliding Window:
    Keeps last K messages; older evicted
    Pros: simple; Cons: forgets important old info
• Block B — Summary + Buffer:
    Old messages summarized into running summary; recent kept verbatim
    Diagram: old msgs → summarizer → condensed slab
• Block C — Hierarchical Memory (MemGPT style):
    Main context + external archival; LLM learns to page in/out
    Diagram: OS-like paging icons
• Small table below comparing token cost / information retention / complexity
```

## 12. 自我反思 & 自我改进

```text
Vertical iterative loop:
• Step 1 — Initial Attempt:
    Agent produces response y_0
• Step 2 — Critique:
    Same or different LLM reviews y_0 for errors
• Step 3 — Refine:
    Agent produces y_1 incorporating critique
• Step 4 — Verify:
    Automated check (unit test / ground truth) — if fail, loop back to Step 2
• Red arrows connecting steps; loop-back arrow from Step 4 to Step 2
• Side chip: "Reflexion · Self-Refine · SELF · Verification-Guided"
• Inline note: "每轮输出的错误被追加进 memory 作为 hints"
```

## 13. Agent Observability / Tracing

```text
Gantt-style trace view:
• X-axis: wall-clock time
• Y-axis: stacked sub-tasks of an agent run (Plan → Tool1 → Reflect → Tool2 → Answer)
• Each row is a horizontal bar colored by category:
    Red: LLM call
    Blue: Tool call
    Gray: Reasoning / state
• Total duration label at top-right: "Run duration: 42 s · 8 LLM calls · $0.12"
• Side panel with top 3 slowest spans highlighted
• Icon chip: "LangSmith · Phoenix · Langfuse"
```

---

## 组合建议

- **"Agent 入门架构"页** → #1 ReAct + #3 Tool Use
- **"复杂 Agent 系统"页** → #2 Planner-Executor + #4 Multi-Agent
- **"记忆与检索"页** → #5 Memory Hierarchy + #6 RAG + #7 Advanced RAG
- **"Context Engineering"页** → #10 Prompt Stack + #11 Memory Policy
- **"Agent 评测 & 自我改进"页** → #9 Radar + #12 Reflection + #13 Trace
- **"三种 Agent 范式对比"页** → #8 Function Calling vs ReAct vs Plan-Execute
