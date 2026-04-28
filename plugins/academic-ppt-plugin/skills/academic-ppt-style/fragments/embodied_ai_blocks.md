# Fragment — 具身智能 (Embodied AI) 可视化块

本 fragment 覆盖 **具身智能 / VLA / 世界模型 / Sim2Real / 机器人学习** 相关视觉组件。与 `fragments/sensor_hardware_renderings.md` 互补（硬件外观在那里，算法架构在这里）。

---

## 1. Perception-Action 闭环

```text
Horizontal closed-loop diagram with 4 stages:
• Stage 1 — Observation (多模态感知):
    Sensor icons row: Camera + LiDAR + IMU + Force/Torque + Microphone
    Outputs: RGB, depth, point cloud, proprioception
• Red arrow → Stage 2 — Perception (状态估计):
    Modules: SLAM · Object Detection · Scene Graph · Language Grounding
• Red arrow → Stage 3 — Planning / Policy (决策):
    Choices: Classical (MPC / TAMP) · Learning-based (RL / IL / Diffusion Policy)
• Red arrow → Stage 4 — Action (动作执行):
    Low-level controller: joint torque / cartesian pose / gripper
• Red return arrow from Stage 4 back to Stage 1 (world state changes, observation loop)
• Center overlay label: "Embodied Agent Loop · 10 Hz ~ 100 Hz"
```

## 2. VLA (Vision-Language-Action) 模型架构

```text
End-to-end VLA diagram, horizontal:
• Left — Multi-Camera Observation:
    Stack of 2-3 camera views (wrist / third-person / top-down)
• Center-Left — Vision Encoder:
    ViT (SigLIP / DINOv2) → visual tokens
• Center — Language Instruction:
    Text "pick up the red cup" → tokenized
• Center-Right — LLM Backbone (7B–30B):
    Accepts interleaved [visual tokens + text tokens] → outputs action tokens
• Right — Action Decoder:
    Three alternative heads shown as a small chip row:
      (a) Discretized tokens (RT-2 style)
      (b) Regression MLP (OpenVLA)
      (c) Diffusion head (π₀ / Octo)
• Below: action output "a_t ∈ ℝ^7 = (Δx, Δy, Δz, Δroll, Δpitch, Δyaw, gripper)"
• Side chip: "训练数据: Open X-Embodiment · 970k trajectories · 22 embodiments"
```

## 3. Diffusion Policy

```text
Diffusion-policy pipeline:
• Left — Conditioning (state / visual encoder output)
• Center — Noise schedule: "a_T ∼ N(0, I) → ... → a_0"
    Visualize as a horizontal denoising strip with gradually cleaner action trajectories
• Right — Predicted action sequence (e.g., 8-step horizon):
    Small plot with 8 time-step markers and a smooth predicted trajectory curve in red
• Small formula box (light red fill):
    "a_{t-1} = (1/√α_t) (a_t − (β_t/√(1−ᾱ_t)) ε_θ(a_t, t, obs))"
• Spec chip: "Action horizon T_a · Observation horizon T_o · CFG scale"
• Note: "开环预测多步动作 + 执行前 k 步 + 重规划 (Receding Horizon)"
```

## 4. Imitation Learning 数据收集

```text
Four-panel collection setup:
• Panel A — Teleoperation:
    Human operator with VR / joystick controls robot arm
    Icon: human + VR goggles
    Pros: high-quality; Cons: slow, expensive
• Panel B — Kinesthetic Teaching:
    Human guides robot arm directly by hand
    Pros: natural contact; Cons: low diversity
• Panel C — Video Demonstration (wild videos):
    Web-scale videos → retargeted actions
    Pros: scalable; Cons: no ground-truth actions
• Panel D — Simulation + Scripted Policies:
    Procedural demos in Isaac / MuJoCo
    Pros: unlimited; Cons: sim-real gap
• Small KPI chip at the bottom: "质量 × 多样性 × 规模 三角权衡"
```

## 5. Reinforcement Learning in Simulation

```text
Sim-training pipeline diagram:
• Left — Simulator (Isaac Sim / Isaac Lab / MuJoCo / Gazebo):
    Shows N parallel environment thumbnails (e.g., 4096 envs)
• Center-Left — Domain Randomization:
    Small dials for lighting / friction / mass / texture with random-shuffle icon
• Center — Policy Network (actor + critic):
    Actor MLP / Transformer; Critic MLP
• Center-Right — PPO / SAC / DreamerV3 training loop:
    Arrows: obs → action → reward → update
• Right — Deployment box (real robot):
    Dashed red arrow labeled "Sim2Real 迁移"
• Spec chip: "并行环境数 4096 · GPU 训练 ~几小时 · 策略上手即用"
```

## 6. Sim2Real Gap 与缓解

```text
Two-pane contrast:
• Left pane — Sim2Real Gap 来源:
    Bullet list (bold red bullets):
      • 物理建模误差 (friction, contact, damping)
      • 视觉域差 (lighting, texture, clutter)
      • 传感器噪声模型不一致
      • Actuator 延迟与带宽
• Right pane — 缓解策略:
    Bullet list:
      • Domain Randomization (DR)
      • Real-to-Sim-to-Real (RSR)
      • System Identification / calibration
      • Adversarial DR / Auto-DR
      • Visual Domain Adaptation (CycleGAN)
      • Residual Policy Learning
• Below both panes: a horizontal arrow "Sim Performance → Real Performance" with a gap indicator showing reduction
```

## 7. 世界模型 (World Model)

```text
Generative world-model architecture:
• Top — Encoder: observation o_t → latent state z_t
• Middle — Dynamics: z_t, a_t → z_{t+1} (predicted next latent)
    Reward predictor: z_t → r_t
• Bottom — Decoder: z_t → reconstructed ô_t
• Policy / Imagination loop on the right:
    Agent rolls out in latent space: z_t → z_{t+1} → ... → z_{t+H}
    Policy trained on imagined trajectories (no env interaction)
• Inline formula (light red fill):
    "ẑ_{t+1} ~ p(z_{t+1} | z_t, a_t),    r̂_t ~ p(r | z_t)"
• Side chip: "DreamerV3 · UniSim · Genie · Sora-as-World-Model"
```

## 8. 机器人技能层级

```text
Three-tier skill hierarchy diagram:
• Tier 1 (top) — High-Level Planner (LLM / VLM):
    Receives natural-language task → outputs sub-goal sequence
    Example: "clean the table" → ["pick up cup", "put in sink", "wipe surface"]
• Tier 2 (middle) — Skill Library (Primitives):
    Grid of learned primitives: pick / place / push / pour / wipe ...
    Each primitive is a pretrained policy (VLA / Diffusion / Scripted)
• Tier 3 (bottom) — Low-Level Controller (Real-time):
    Joint-torque / impedance / cartesian impedance control at 500-1000 Hz
• Red arrows downward for command flow, gray upward for state feedback
• Side chip: "LLM 作为 meta-controller · Code-as-Policies · VoxPoser"
```

## 9. 任务与运动规划 (TAMP)

```text
Dual-layer planning diagram:
• Upper layer — Symbolic Task Planner:
    STRIPS-like state transition graph
    Nodes: (on cup table), (in cup sink), etc.
    Goal: reach sink-state
• Lower layer — Motion Planner:
    Configuration space with obstacles; RRT / BIT* trajectory shown as a red curve
    Collision-free path between key poses
• Bridging arrows: each symbolic action (e.g., pick(cup)) expands into a motion-planning problem
• Small chip: "PDDLStream · LLM-TAMP · SayCan"
```

## 10. 语言条件策略 (Language-Conditioned Policy)

```text
Policy network with language input:
• Left: observation (image) → visual encoder → z_v
• Middle-Left: instruction text "put the yellow block on the tray" → text encoder → z_l
• Middle: cross-attention fusion → conditioned state z
• Right: policy head → action a_t
• Below: dataset note "CALVIN · RT-1 · BridgeData · Libero"
• Small badge: "语义泛化 · 跨任务迁移"
```

## 11. 机器人数据收集与基准

```text
Horizontal timeline of major embodied datasets:
• RT-1 (2022) — 130k episodes · Google Kitchen
• RH20T (2023) — 110k episodes · 147 tasks
• Open X-Embodiment (2023) — 970k trajectories · 22 embodiments
• DROID (2024) — 76k trajectories · 564 scenes
• RoboMIND (2024) — 107k episodes
• Each entry shown as a small chip card with year + key statistic
• Arrow line showing growth trend
• Side chip: "数据量增长 ~10x / 2 年"
```

## 12. 具身评测基准

```text
Hexagonal radar chart for embodied agent evaluation:
• 6 axes:
  – 长程任务成功率 (Long-Horizon Success Rate)
  – 泛化能力 (Out-of-Distribution)
  – 鲁棒性 (Disturbance Recovery)
  – 效率 (Time / Energy)
  – 安全性 (Collision / Failure)
  – 模仿保真度 (Imitation Fidelity)
• Overlaid polygons: red = ours, gray = baseline
• Side: benchmark chip list "CALVIN · LIBERO · SimplerEnv · ManiSkill · Open6DOR · RoboCasa"
```

## 13. 真机部署架构

```text
Left-to-right deployment stack:
• Edge compute unit (Jetson Orin / x86 GPU):
    Runs perception + policy inference
• Middleware: ROS2 / DDS
    Topics: /image, /joint_state, /action, /tf
• Real-time controller (EtherCAT / CAN):
    1 kHz joint control loop
• Robot hardware:
    Manipulator / quadruped / humanoid icon
• Arrows:
    Red downward for command (action at 10-30 Hz from policy → 1 kHz interp)
    Gray upward for feedback
• Side chip: "异步推理 · 时间同步 · 安全监控 · Emergency Stop"
```

---

## 组合建议

- **"具身基础模型"页** → #2 VLA + #3 Diffusion Policy + #7 World Model
- **"机器人学习范式"页** → #4 Imitation + #5 RL + #6 Sim2Real
- **"分层控制"页** → #1 闭环 + #8 Skill Hierarchy + #9 TAMP
- **"数据与评测"页** → #11 Datasets + #12 Radar
- **"真机部署"页** → #13 Deployment + `sensor_hardware_renderings.md` 的具体硬件
- **"语言驱动具身"页** → #10 Language-Conditioned + #8 Skill Hierarchy
