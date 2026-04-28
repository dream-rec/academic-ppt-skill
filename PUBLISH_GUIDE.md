# 发布步骤

## 1. 初始化Git仓库

```bash
cd /data/workspace/academic-ppt-marketplace
git init
git add .
git commit -m "Initial commit: Academic PPT Style plugin"
```

## 2. 创建GitHub仓库

1. 访问 https://github.com/new
2. 创建一个新仓库，例如命名为 `academic-ppt-marketplace`
3. 不要初始化README、.gitignore或license（我们已经有了）

## 3. 推送到GitHub

```bash
# 替换为你的GitHub用户名和仓库名
git remote add origin https://github.com/YOUR_USERNAME/academic-ppt-marketplace.git
git branch -M main
git push -u origin main
```

## 4. 用户安装方式

其他用户可以通过以下命令安装：

```bash
# 添加你的marketplace
/plugin marketplace add YOUR_USERNAME/academic-ppt-marketplace

# 安装plugin
/plugin install academic-ppt-style@academic-ppt-marketplace
```

## 5. 使用方式

安装后，用户可以这样使用：

```
/academic-ppt-style:academic-ppt-style 帮我生成一张学术PPT的提示词，主题是XXX
```

## 6. 提交到官方marketplace（可选）

如果想让更多人发现你的plugin，可以提交到Anthropic官方marketplace：

- Claude.ai: https://claude.ai/settings/plugins/submit
- Console: https://platform.claude.com/plugins/submit

## 目录结构说明

```
academic-ppt-marketplace/
├── marketplace.json              # marketplace配置文件
├── PUBLISH_GUIDE.md              # 本文件
└── plugins/
    └── academic-ppt-plugin/
        ├── .claude-plugin/
        │   └── plugin.json       # plugin元数据
        ├── skills/
        │   └── academic-ppt-style/  # 你的skill
        │       ├── SKILL.md
        │       ├── templates/
        │       ├── fragments/
        │       └── examples/
        └── README.md             # plugin说明文档
```

## 注意事项

1. 记得在 [plugin.json](plugins/academic-ppt-plugin/.claude-plugin/plugin.json) 中修改 `author` 字段为你的名字
2. 记得在 [marketplace.json](marketplace.json) 中修改 `owner.name` 字段
3. 记得在 [README.md](plugins/academic-ppt-plugin/README.md) 中更新GitHub用户名和作者信息
4. 推送前确保所有敏感信息已移除
