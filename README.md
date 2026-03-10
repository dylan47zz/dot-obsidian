# Obsidian 双库配置共享方案

> 使用符号链接实现多个 Obsidian 库共享同一套配置，同时保持笔记内容隔离。

## 方案概述

本方案实现了一个 Obsidian 双库（个人库 + 工作库）配置共享的架构：

- **mems-garden** - 个人知识库
- **my-work** - 工作知识库

两个库共享同一套配置模板，修改一次配置即可同步到所有库，无需重复配置。

## 目录结构

```
~/my-all/notes/
├── .obsidian-config/          # 共享配置模板（Git 托管）
│   ├── .git/                  # Git 仓库
│   ├── .gitignore
│   ├── core-plugins.json      # 核心插件配置
│   ├── community-plugins.json # 社区插件列表
│   ├── hotkeys.json           # 快捷键配置
│   ├── graph.json             # 关系图配置
│   ├── themes/                # 共享主题
│   │   └── Baseline/
│   └── plugins/               # 共享插件
│       ├── claudian/
│       ├── editing-toolbar/
│       ├── obsidian-tasks-plugin/
│       └── ...
│
├── mems-garden/               # 个人知识库
│   └── .obsidian/
│       ├── app.json           # 私有配置
│       ├── workspace.json     # 私有配置
│       ├── appearance.json    # 私有配置（主题）
│       ├── daily-notes.json   # 私有配置
│       ├── types.json         # 私有配置
│       ├── core-plugins.json → ../../.obsidian-config/core-plugins.json
│       ├── community-plugins.json → ../../.obsidian-config/community-plugins.json
│       ├── hotkeys.json → ../../.obsidian-config/hotkeys.json
│       ├── graph.json → ../../.obsidian-config/graph.json
│       ├── themes → ../../.obsidian-config/themes
│       └── plugins → ../../.obsidian-config/plugins
│
└── my-work/                   # 工作知识库
    └── .obsidian/
        ├── (私有配置同上)
        └── (符号链接同上)
```

## 配置共享说明

| 配置类型 | 文件 | 共享方式 |
|----------|------|----------|
| 核心插件 | `core-plugins.json` | ✅ 符号链接共享 |
| 社区插件 | `community-plugins.json` | ✅ 符号链接共享 |
| 快捷键 | `hotkeys.json` | ✅ 符号链接共享 |
| 关系图 | `graph.json` | ✅ 符号链接共享 |
| 主题文件 | `themes/` | ✅ 符号链接共享 |
| 插件代码 | `plugins/` | ✅ 符号链接共享 |
| 主题选择 | `appearance.json` | 🔒 私有（每个库可设置不同主题） |
| 工作区布局 | `workspace.json` | 🔒 私有 |
| 每日笔记 | `daily-notes.json` | 🔒 私有 |
| 应用配置 | `app.json` | 🔒 私有 |
| 类型定义 | `types.json` | 🔒 私有 |

**设计原则**：视觉相关配置（如主题）保持私有，便于快速区分不同库；功能性配置共享，减少重复工作。

## 仓库信息

- **GitHub 仓库**: https://github.com/dylan47zz/dot-obsidian
- **本地路径**: `~/my-all/notes/.obsidian-config/`

---

## 使用教程

### 1. 克隆配置仓库到新设备

在另一台设备上，克隆配置仓库：

```bash
# 克隆配置仓库
git clone git@github.com:dylan47zz/dot-obsidian.git ~/.obsidian-config
```

### 2. 创建新的 Obsidian 库

```bash
# 创建库目录
mkdir -p ~/my-vault/.obsidian

# 进入配置目录
cd ~/.obsidian-config

# 创建符号链接（macOS/Linux）
ln -s ~/.obsidian-config/core-plugins.json ~/my-vault/.obsidian/core-plugins.json
ln -s ~/.obsidian-config/community-plugins.json ~/my-vault/.obsidian/community-plugins.json
ln -s ~/.obsidian-config/hotkeys.json ~/my-vault/.obsidian/hotkeys.json
ln -s ~/.obsidian-config/graph.json ~/my-vault/.obsidian/graph.json
ln -s ~/.obsidian-config/themes ~/my-vault/.obsidian/themes
ln -s ~/.obsidian-config/plugins ~/my-vault/.obsidian/plugins
```

### 3. 配置私有文件

每个库需要创建以下私有配置文件：

```bash
# 进入库的配置目录
cd ~/my-vault/.obsidian

# 创建 appearance.json（主题选择，根据需要修改）
echo '{"cssTheme": "Baseline"}' > appearance.json

# 从示例创建其他私有配置
# 这些配置通常需要在新库中重新设置
touch app.json workspace.json types.json daily-notes.json
```

### 4. 同步配置更新

当修改了共享配置后，提交并推送：

```bash
cd ~/.obsidian-config

# 添加更改
git add -A

# 提交
git commit -m "feat: update config description"

# 推送到 GitHub
git push
```

在其他设备上拉取更新：

```bash
cd ~/.obsidian-config
git pull
```

---

## 常见问题

### Q: 如何为不同库设置不同主题？

修改每个库的 `appearance.json` 文件：

```json
{
  "cssTheme": "你的主题名称"
}
```

主题文件本身是共享的，只需在 `appearance.json` 中指定不同的主题名称即可快速区分库。

### Q: 添加新插件后如何同步？

1. 在一个库中安装插件
2. 将插件文件夹复制到 `.obsidian-config/plugins/`
3. 提交推送到 GitHub
4. 在其他设备上拉取更新

### Q: 为什么有些配置是私有的？

- **appearance.json**: 允许不同库使用不同主题，一眼区分库
- **workspace.json**: 包含当前打开的文件，库之间不同
- **daily-notes.json**: 每日笔记的文件夹路径不同
- **plugins/*/data.json**: 运行时数据，不同库独立

### Q: 如何更新插件？

更新插件有三种方式：

1. **直接更新共享插件**：在新库中更新插件，然后复制到 `.obsidian-config/plugins/`
2. **各个库独立更新**：删除符号链接，在库中单独安装插件
3. **手动同步**：复制对应插件文件夹到共享目录

---

## 维护指南

### 添加新的共享配置

```bash
# 1. 在共享目录添加配置
cp ~/my-vault/.obsidian/new-config.json ~/.obsidian-config/

# 2. 在各库创建符号链接
ln -s ~/.obsidian-config/new-config.json ~/my-vault/.obsidian/new-config.json

# 3. 提交推送
git add new-config.json
git commit -m "feat: add new shared config"
git push
```

### 备份配置

配置仓库已包含 Git 版本控制，推送到 GitHub 即是最好的备份。

### 重置配置

如果配置出错，可以从 GitHub 恢复：

```bash
cd ~/.obsidian-config
git fetch origin
git reset --hard origin/main
```

---

## 更新日志

- **2026-03-10**: 初始化方案，创建双库配置共享架构
- 包含 8 个社区插件和 1 个主题的共享配置
