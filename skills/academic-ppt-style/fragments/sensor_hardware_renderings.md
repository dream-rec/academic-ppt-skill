# Fragment — 硬件渲染提示词（常见硬件）

## 用途

文生图模型对"相机 / 激光雷达 / 气体传感"等通用名词的外观生成往往是**通用图标**或**编造产品**，导致不贴近真实汇报场景。本 fragment 给出**固定外观描述**，确保任何时候引用某个具体型号都能复现一致的形态。

使用方法：在 Module / Card 里需要渲染该硬件时，**原样嵌入**对应片段。

---

## 1. Unitree B2 四足机器人

```text
Realistic 3/4 side-perspective vector-style illustration of the Unitree B2 quadruped robot:
• Industrial matte-black + dark-gray chassis with yellow accent lines along the torso edges
• Four articulated legs, each with visible hip-knee-ankle joint actuators (12 joints total, 3×4 configuration)
• Robust rectangular body; top deck flat for payload mounting (payload bay)
• Head module at the front end (left of the robot in this view), small rectangular housing for sensors
• Rear end has a slight taper housing the compute unit
• Subtle grid shadow on the ground beneath the robot
• Slightly crouched stance to convey stability
• No cartoon styling; technical product illustration feel
```

## 2. Stereolabs ZED 2 双目立体相机

```text
Realistic rendering of a Stereolabs ZED 2 stereo camera:
• Slim black aluminum rectangular bar, aspect ~6:1 horizontally
• Two round lens apertures on the front face, evenly spaced, with subtle lens reflections
• Brushed aluminum finish with thin chamfered edges
• Small USB-C cable tail exiting from one end
• No logo text; industrial product-photo style
```

## 3. Livox Mid-360 三维激光雷达

```text
Realistic rendering of a Livox Mid-360 LiDAR:
• Compact black cylindrical puck shape, diameter ~65mm, height ~60mm
• Silver/chrome reflective aperture ring on the top face (scanning optics)
• Black matte side housing with small ventilation slots
• Short pigtail cable exiting from the bottom
• Clean industrial product rendering
• When visualizing its scan pattern, use a non-repetitive flower/rose-petal point cloud (NOT the rotating 360° line pattern of mechanical LiDARs)
```

## 4. 四合一气体传感阵列 (CH₄ / CO / H₂S / O₂)

```text
Realistic rendering of a four-in-one multi-gas detector module:
• Rectangular black plastic enclosure, approximately 80×60×25 mm
• Perforated metal grille covering most of the front face, with four labeled sensor head openings marked "CH₄", "CO", "H₂S", "O₂"
• A small green status LED indicator on one corner
• Short connector cable (RS-485 / M8) exiting from one side
• Industrial / intrinsically-safe product look; NOT a consumer gas detector
• Do NOT render it as a set of dashboard gauges; the hardware itself must be visually distinct
• Separately, beneath the hardware rendering, you MAY add a 2×2 grid of compact circular gauge chips (CH₄, CO, H₂S, O₂ with green/yellow/red threshold arcs) — the gauges are visualization, the module above is hardware
```

## 5. NVIDIA Jetson Orin 边缘计算单元

```text
Realistic rendering of an NVIDIA Jetson Orin developer kit module:
• Compact black rectangular aluminum heatsink with dense heat-sink fins visible on top
• Side IO: USB-A × 4, HDMI, Gigabit Ethernet, DC power barrel connector
• Dimensions ~100×80×50 mm
• Industrial embedded-computing product look
```

## 6. 通用 RGB 相机（不指定具体型号时）

```text
Realistic rendering of an industrial machine-vision camera:
• Compact cubic aluminum body (~40×40×40 mm)
• Single central lens with a threaded C-mount; lens visible with subtle reflections
• Gigabit Ethernet / USB 3.0 connector at the rear
• Matte black or silver-anodized finish
• Status LED on the rear
```

## 7. 机械式 360° 激光雷达（如 Velodyne / RS-LiDAR-16）

```text
Realistic rendering of a mechanical rotating 360° LiDAR:
• Cylindrical shape ~100mm diameter, ~80mm height
• Black matte housing with a transparent rotating aperture band around the middle
• Mounting flange on the bottom
• Short cable pigtail
• When visualizing scan pattern, use horizontal ring-like layers of points
```

## 8. IMU + GNSS 模块

```text
Realistic rendering of an IMU + dual-frequency RTK GNSS module:
• Small sealed black rectangular enclosure (~70×50×15 mm)
• Two SMA antenna connectors on top
• Multi-pin M8/M12 connector on one side
• Industrial / automotive-grade appearance
```

## 9. 隧道内景（点云）可视化

```text
Long curved tunnel interior rendered as a dense colored 3D point cloud:
• Horseshoe / circular-arch cross-section clearly visible
• Warm color (orange / red) gradient for points near the camera, cool color (cyan / blue) gradient for points far in the distance
• Soft perspective convergence toward a vanishing point
• Optional overlay: a semi-transparent RGB thumbnail frame in the lower-right, with a thin red dashed camera frustum projecting onto the point cloud, illustrating pixel-level image-to-point-cloud registration
• Optional overlay: semi-transparent red polygonal regions on the tunnel lining with small labels such as "衬砌开裂区" · "渗漏水斑" · "施工缝"
```

## 10. 隧道断面几何图（工程制图风）

```text
Engineering-style horseshoe tunnel cross-section drawing:
• Outer profile: horseshoe arch with 30–40 cm thick concrete lining
• Shotcrete texture hint on the inner surface
• Dimension lines with arrows and labels: "B = 12.0 m" (span), "H = 10.5 m" (height), "R = 6.5 m" (arch radius)
• Thin black linework on white background, blueprint-style but limited to red-gray academic palette
```

---

## 使用惯例

- 当模块里同时出现多个硬件时（例如三沙传感器对比卡），**每个 card 顶部都贴该硬件的完整渲染描述**，并在下方加统一格式的 specs 行。
- 若用户给出不同型号，**复制本 fragment 的某一节作为模板**，替换外形描述，保持其他格式不变。
- 渲染**必须具备真实产品辨识度**，不要做成"相机图标"或"激光雷达图标"。
