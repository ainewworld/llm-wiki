# LLM Wiki

一个用于构建 **LLM 驱动知识库** 的开源模板，遵循 [Andrej Karpathy 的“LLM Wiki”模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)。

你只需要提供原始资料。LLM 会读取这些资料，编写结构化的 Wiki 页面、建立交叉链接，并随着时间推移持续维护整个知识库。你永远不需要直接编辑 Wiki——你只负责整理资料并提出问题。

## 工作原理

整个系统分为三层：

```text
raw/              你收集的原始资料（文章、访谈记录、笔记、PDF 等）
wiki/             LLM 编写并维护的页面（摘要、概念、实体、综合分析）
CLAUDE.md         定义 Wiki 结构的 Schema，指导 LLM 如何组织内容
```

整个工作流由三个操作驱动：

| 操作             | 触发方式                       | 执行内容                                                         |
| -------------- | -------------------------- | ------------------------------------------------------------ |
| **Ingest（导入）** | `ingest raw/my-source.txt` | LLM 读取资料，创建摘要页面，创建或更新概念页和实体页，添加交叉链接，并更新索引和日志                 |
| **Query（查询）**  | 提出任意问题                     | LLM 搜索 Wiki，综合整理答案并附带引用；如产生新的洞见，可自动创建综合分析页面                  |
| **Lint（检查）**   | `lint` 或 `health check`    | LLM 审查所有页面，查找孤立页面、内容矛盾、缺失链接、不完整章节和低可信度声明；能自动修复的立即修复，其余问题生成报告 |

## 快速开始

1. **克隆仓库**

   ```bash
   git clone https://github.com/YOUR_USERNAME/llm-wiki.git my-knowledge-base
   cd my-knowledge-base
   ```

2. **根据你的领域修改 `CLAUDE.md`**

   * 更新 **Purpose（用途）** 部分，描述你的知识领域
   * 将占位符标签体系替换为你自己的分类
   * 根据需要调整可信度等级说明
   * 其余内容（工作流、页面格式、链接规则等）可直接使用

3. **将资料放入 `raw/`**

   * 支持文本文件、访谈记录、文章、笔记等任意纯文本
   * 一旦加入，这些原始资料即保持不可变，LLM 不会修改它们

4. **让 LLM 导入资料**

   ```text
   ingest raw/my-first-source.txt
   ```

   LLM 将创建摘要页面、概念页面、实体页面、交叉链接，并更新索引。

5. **提出问题**

   ```text
   X 和 Y 的关键区别是什么？
   ```

   LLM 将根据 Wiki 内容回答，并引用相关页面。

6. **运行健康检查**

   ```text
   lint
   ```

   LLM 会检查整个 Wiki 并修复存在的问题。

## 目录结构

```text
.
├── CLAUDE.md                      # Schema——指导 LLM 的规则
├── raw/                           # 原始资料（不可修改）
└── wiki/
    ├── index.md                   # 所有页面的主索引
    ├── log.md                     # 追加式活动日志
    ├── dashboard.md               # Dataview 仪表盘（Obsidian）
    ├── analytics.md               # Charts View 数据分析（Obsidian）
    ├── flashcards.md              # 间隔重复学习卡片
    ├── summaries/                 # 每个源文档对应一个摘要页面
    ├── concepts/                  # 概念与框架页面
    ├── entities/                  # 人物、工具、组织等实体页面
    ├── syntheses/                 # 跨主题综合分析
    ├── journal/                   # 研究/学习日志
    │   └── template.md            # 日志模板
    └── presentations/             # Marp 演示文稿
```

## 增强功能

除了核心 Wiki 模式之外，此模板还包含以下扩展功能：

### Dataview Dashboard（`wiki/dashboard.md`）

通过实时查询展示低可信度页面、最近更新、按标签分类的概念，以及引用来源最多的页面。

需要安装 Obsidian 的 **Dataview** 插件。

### Charts View Analytics（`wiki/analytics.md`）

提供饼图、柱状图、词云等可视化分析。

需要安装 Obsidian 的 **Charts View** 插件。

### Mermaid 图表

可在任意 Wiki 页面中使用 Mermaid 代码块创建流程图、时序图或概念图。

Obsidian 与 GitHub 原生支持。

### Marp 幻灯片（`wiki/presentations/`）

使用 Markdown 创建演示文稿。

只需将演示文件放入该目录即可通过 **Marp** 生成幻灯片。

### Research Journal（`wiki/journal/`）

用于记录研究过程、实验或实践工作，并附带模板。

LLM 在回答问题时也可以引用这些日志内容。

### Spaced Repetition（`wiki/flashcards.md`）

按照 Obsidian **Spaced Repetition** 插件格式编写的学习卡片。

可以要求 LLM 根据任意 Wiki 页面自动生成复习卡片。

### MCP Server

该仓库支持 Claude Code 的 MCP Server 功能。

将 MCP 兼容客户端指向此仓库后，LLM 即可通过程序方式读写整个 Wiki。

## 针对你的领域进行定制

`CLAUDE.md` 中的 Schema 与具体领域无关，可按以下方式进行调整：

1. **Purpose（用途）** —— 用一段文字描述你的知识领域
2. **标签体系（Tagging taxonomy）** —— 用自己的分类替换占位符（例如烹饪知识库可使用：`菜系`、`烹饪技巧`、`食材`、`厨具`）
3. **可信度等级（Confidence levels）** —— 根据你的领域调整证据标准
4. **实体类型（Entity types）** —— 修改实体页面定义，使其符合你的领域（人物、工具、公司等）
5. **日志模板（Journal template）** —— 根据自己的工作流修改 `wiki/journal/template.md`

除此之外，页面格式、链接规范、工作流以及规则都是通用的，适用于各种知识领域。

## 示例应用领域

该模板适用于任何知识密集型主题，例如：

* **研究笔记** —— 论文、实验、研究方法
* **图书分析** —— 主题、人物、作者写作技巧
* **竞品分析** —— 公司、产品、市场趋势
* **课程笔记** —— 课堂内容、阅读材料、核心概念
* **个人成长** —— 框架、习惯、读书笔记
* **技术文档** —— API、系统架构、设计模式
* **兴趣专题研究** —— 任何你希望深入掌握的主题

## 许可证

MIT
