# Academic PPT Style Plugin

生成红灰配色、纯白背景的学术工程风格 PPT 幻灯片图像提示词。

## 功能特点

- 🎨 红灰双色 + 纯白背景的学术风格
- 📊 支持多种模板（顶部横幅式、左上标题式、自适应式）
- 🔬 适用于 SCI 期刊图 / 国家重点研发项目评审级别的汇报
- 🤖 支持多个AI领域：LLM、VLM、Agent、具身智能等
- 📐 输出可直接用于文生图模型的结构化提示词

## 安装方法

### 通过marketplace安装（推荐）

```bash
# 添加marketplace
/plugin marketplace add dream-rec/academic-ppt-skill

# 安装plugin
/plugin install academic-ppt-style@academic-ppt-marketplace
```

### 本地安装

```bash
# 克隆仓库
git clone https://github.com/dream-rec/academic-ppt-skill.git

# 在Claude Code中添加本地plugin
/plugin add /path/to/academic-ppt-skill
```

### 安装后验证

```bash
/plugin list
/plugin inspect academic-ppt-style
```

## 使用方法

安装后，可以通过以下方式使用：

```
帮我生成一张学术PPT提示词，主题是"长大隧道多模态感知"
```

或者直接描述需求：

```
帮我生成一张红灰配色的学术风格PPT，主题是"长大隧道多模态感知"
```

## 支持的场景

- 技术路线图
- 成果展示页
- 关键科学问题页
- 学术汇报页
- 系统架构图

## Windows 常见问题

### 1) `install` 提示已安装，但 `discover` 看不到

这是常见现象，`discover` 不一定展示所有 skill 型插件。  
以以下命令结果为准：

```bash
/plugin list
/plugin inspect academic-ppt-style
```

并直接发送需求进行触发测试。

### 2) `/plugin inspect academic-ppt-style` 空内容

通常是 scope 不一致或缓存旧版本导致。请用 CLI 排查并重装：

```bash
claude plugin list --json
claude plugin uninstall academic-ppt-style@academic-ppt-marketplace --scope user
claude plugin uninstall academic-ppt-style@academic-ppt-marketplace --scope project
claude plugin uninstall academic-ppt-style@academic-ppt-marketplace --scope local
claude plugin install academic-ppt-style@academic-ppt-marketplace --scope user
```

### 3) 更新仓库后本地不生效

当 `plugin.json` 的 `version` 未变化时，客户端可能继续使用缓存版本。  
建议每次发布都递增 `version`，并执行：

```bash
/plugin update academic-ppt-style@academic-ppt-marketplace
```

## 版本历史

- v1.0.1: 显式声明 skills 路径并提升插件兼容性
- v1.0.0: 初始版本

## 许可证

MIT

## 作者

dream-rec
