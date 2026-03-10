# Obsidian 知识库设计方案

> 基于 kepano (Steph Ango) 的个人知识管理方法
> 设计日期：2026-03-10

---

## 一、设计理念

### kepano 核心原则

| 原则 | 说明 |
|------|------|
| **避免文件夹** | 笔记属于多个主题，不被单一文件夹限制 |
| **使用属性组织** | 用 properties + 标签来分类 |
| **大量内部链接** | 第一次提到就链接，未解析的链接是未来连接的线索 |
| **日期格式** | YYYY-MM-DD |
| **标签用复数** | `books` 而非 `book` |
| **根目录为主** | 大多数笔记放在统一目录 |
| **tasknotes** | 每个任务一个独立笔记 |

### 本方案自定义规则

- **笔记目录**：所有笔记存放在 `笔记/` 目录
- **category 属性**：使用 `category` 属性区分笔记类型，替代文件夹
- **任务独立目录**：`任务/` 目录存放 tasknotes

---

## 二、目录结构

### mems-garden（个人知识库）

```
mems-garden/
├── 笔记/                   # 所有笔记（无子文件夹，用 category 区分）
├── 任务/                   # tasknotes：每个任务一个笔记
├── 索引/                   # Bases 分类数据库
├── 剪藏/                   # 保存他人的文章
├── 日记/                   # 每日笔记（YYYY-MM-DD.md），仅用于链接
├── 附件/                   # 图片、音频、视频、PDF
└── 模板/                   # 模板文件
```

### my-work（工作知识库）

```
my-work/
├── 笔记/                   # 所有笔记
├── 任务/                   # tasknotes
├── 索引/                   # Bases 数据库
├── 剪藏/                   # 保存的技术文章
├── 日记/                   # 每日笔记
├── 附件/                   # 图片、截图、PDF
└── 模板/                   # 模板文件
```

---

## 三、目录说明

### 3.1 笔记目录

存放所有笔记，无子文件夹，完全依靠 `category` 属性区分类型。

### 3.2 任务目录

采用 tasknotes 理念，每个任务一个独立笔记，便于：
- 详细记录任务背景和思考
- 任务执行过程中的笔记
- 任务复盘

### 3.3 索引目录

存放 Obsidian Bases 数据库文件，用于分类检索。

### 3.4 剪藏目录

保存从网页等渠道收集的他人文章/内容。

### 3.5 日记目录

仅存放 YYYY-MM-DD.md 文件，作为链接枢纽，不直接在里面写内容。

### 3.6 附件目录

存放图片、音频、视频、PDF 等媒体文件。

### 3.7 模板目录

存放各种笔记模板。

---

## 四、命名规范

### 4.1 笔记命名

| 类型 | 格式 | 示例 |
|------|------|------|
| 日记碎片 | `YYYY-MM-DD.md` | `2026-03-10.md` |
| 一般笔记 | `描述性标题.md` | `关于学习的思考.md` |
| 引用笔记 | `名称.md` | `盗梦空间.md`、`老婆.md` |

### 4.2 任务命名

```
日期-任务标题.md
2026-03-10-日本旅行计划.md
2026-03-10-项目A-需求分析.md
```

### 4.3 索引命名

```
名称.base.yaml
books.base.yaml
movies.base.yaml
```

### 4.4 模板命名

```
类型-模板.md
日记模板.md
书籍模板.md
任务模板.md
```

---

## 五、属性规范

### 5.1 通用属性

```yaml
---
created: 2026-03-10      # 创建日期 YYYY-MM-DD
updated: 2026-03-10      # 更新日期 YYYY-MM-DD
tags: []                  # 标签（复数）
category:                # 分类（见下方分类表）
status:                  # 状态
---
```

### 5.2 mems-garden category

| category | 说明 |
|----------|------|
| `journal` | 每日日记碎片 |
| `thought` | 想法、思考 |
| `opinion` | 观点、感悟 |
| `books` | 书籍 |
| `movies` | 电影 |
| `podcasts` | 播客 |
| `places` | 地点 |
| `people` | 人物 |
| `quotes` | 引言 |

### 5.3 my-work category

| category | 说明 |
|----------|------|
| `journal` | 每日工作日志 |
| `tech` | 技术笔记 |
| `thought` | 工作想法 |
| `project` | 项目相关 |
| `meeting` | 会议记录 |
| `doc` | 文档 |

### 5.4 状态属性

| status | 说明 |
|--------|------|
| `pending` | 待处理 |
| `in-progress` | 进行中 |
| `done` | 已完成 |
| `cancelled` | 已取消 |
| `reading` | 阅读中（书籍/文章） |
| `completed` | 已完成（阅读） |

### 5.5 评分属性（7分制）

| 评分 | 含义 |
|------|------|
| 7 | 完美，必须体验 |
| 6 | 优秀，值得重复 |
| 5 | 不错 |
| 4 | 一般 |
| 3 | 糟糕 |
| 2 | 极差 |
| 1 | 极差（负向） |

---

## 六、模板设计

### 6.1 日记模板（mems-garden）

```yaml
---
created: {{date}}
tags: [journal, daily]
category: journal
---
# {{date}}

# 今日完成

# 今日思考

# 今日感恩

# 明日待办
- [ ]
```

### 6.2 书籍模板

```yaml
---
created: {{date}}
tags: [books, {{type}}]
category: books
author:
year:
rating:
status: reading
genre:
---
# {{title}}

## 概要

## 核心观点

## 摘录

## 思考

## 相关笔记
```

### 6.3 电影模板

```yaml
---
created: {{date}}
tags: [movies, {{type}}]
category: movies
director:
year:
rating:
status: watched
genre:
watched-with:
place:
---
# {{title}}

## 概要

## 观后感

## 精彩片段

## 相关笔记
```

### 6.4 人物模板

```yaml
---
created: {{date}}
tags: [people]
category: people
relation:
meet-date:
rating:
---
# {{name}}

## 基本信息

## 相识经过

## 特点

## 共同回忆

## 相关笔记
```

### 6.5 任务模板

```yaml
---
created: {{date}}
tags: [tasks, {{type}}]
category: tasks
status: pending
priority: medium
due:
start:
end:
---
# {{title}}

## 背景

## 目标

## 执行计划

## 进度

## 复盘

## 相关笔记
```

### 6.6 技术笔记模板（my-work）

```yaml
---
created: {{date}}
tags: [tech, {{type}}]
category: tech
status:
---
# {{title}}

## 背景

## 核心内容

## 示例

## 参考资料

## 相关笔记
```

---

## 七、使用示例

### 7.1 写日记

```
1. 使用快速创建笔记快捷键
2. 自动生成 YYYY-MM-DD-HHmm.md 文件名
3. 内容直接写入笔记
4. 链接到其他地方
```

### 7.2 记录书籍

```
1. 从模板创建笔记
2. 填写书籍基本信息（属性）
3. 写入读书笔记
4. 链接到其他相关笔记
```

### 7.3 链接示例

在日记中链接到书籍：

```markdown
今天开始读 [[Atomic Habits]]，这本书是 James Clear 写的...
```

在日记中链接到电影：

```markdown
周末看了 [[盗梦空间]]，和 [[老婆]] 一起在 [[万达影院]] 看的...
```

---

## 八、 Bases 索引设计

### 8.1 books.base.yaml

```yaml
type: database
views:
  - type: table
    columns:
      - property: title
      - property: author
      - property: rating
      - property: genre
      - property: status
      - property: created
```

### 8.2 movies.base.yaml

```yaml
type: database
views:
  - type: table
    columns:
      - property: title
      - property: director
      - property: rating
      - property: genre
      - property: watched-with
      - property: place
```

---

## 九、工作流建议

### 9.1 每日流程

1. **早晨**：查看今日任务
2. **工作中**：记录想法、技术笔记
3. **每日回顾**：写日记碎片，链接相关笔记

### 9.2 定期回顾

- **每几天**：整理日记碎片，整合到月度笔记
- **每月**：月度回顾
- **每年**：年度回顾

### 9.3 Random Revisit

每隔几个月，使用随机笔记功能浏览旧笔记，发现遗漏的链接，创建新的连接。

---

## 十、附录

### 10.1 按键快捷键建议

| 操作 | 快捷键 |
|------|--------|
| 快速创建笔记 | Ctrl/Cmd + N |
| 快速切换 | Ctrl/Cmd + O |
| 插入内部链接 | Ctrl/Cmd + [ |
| 今日日记 | 自定义 |

### 10.2 标签规范

```
# 个人库
#journal #thought #opinion
#books #movies #podcasts #places #people #quotes
#tasks #habits

# 工作库
#journal #tech #thought #project #meeting #doc
#tasks #code
```

---

## 更新日志

- **2026-03-10**: 初始版本，基于 kepano 方法设计
