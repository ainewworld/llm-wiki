# [你的领域] 知识库 —— Schema

## 用途（Purpose）

<!-- 自定义：请将此部分替换为你所研究领域的一段描述。 -->

<!-- 示例："机器学习研究"、"19 世纪文学"、"SaaS 工具竞争格局" -->

这是一个由 **LLM 维护** 的 **[你的主题]** 知识库。LLM 负责编写和维护 `wiki/` 下的所有文件；人类负责整理原始资料（raw sources）并提出查询请求。人类**永远不直接编辑** `wiki/` 中的文件。

## 目录结构（Directory Layout）

* `raw/` —— 不可修改（Immutable）的原始资料（访谈、文章、笔记等）。**绝不能修改这些文件。**
* `wiki/index.md` —— 主索引。**所有 Wiki 页面都必须出现在这里。**
* `wiki/log.md` —— 追加式（Append-only）活动日志。
* `wiki/summaries/` —— 每个原始资料对应一个摘要页面。
* `wiki/concepts/` —— 概念、策略和框架页面。
* `wiki/entities/` —— 实体页面（人物、工具、组织、产品——即你领域中的各种“对象”）。
* `wiki/syntheses/` —— 对比分析、决策框架和跨主题综合分析。
* `presentations/`（仓库根目录，可选）—— 幻灯片或其他衍生材料。**凡是不属于知识内容的文件，都不要放进 `wiki/`；`wiki/` 仅保存正式发布的知识内容。**

## 文件命名（File Naming）

* 全部使用小写字母
* 单词之间使用连字符（`-`）分隔，例如：`concept-name.md`
* 不允许使用空格、特殊字符或大写字母
* 文件名应与页面标题对应的 slug 保持一致

## 页面格式（Page Format）

每个 Wiki 页面都必须使用如下 Frontmatter：

```yaml
---
title: "页面标题"
type: concept | entity | summary | synthesis
tags: [tag1, tag2, tag3]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["raw/filename.txt"]
confidence: high | medium | low
---
```

## 各类页面必须包含的章节

### 摘要页面（`wiki/summaries/`）

必须包含以下章节：

* `## Key Points` —— 主要观点/核心内容（项目符号列表）
* `## Relevant Concepts` —— 本资料涉及的概念页面链接
* `## Source Metadata` —— 来源信息（资料类型、作者/演讲者、日期、URL 或其他标识）

---

### 概念页面（`wiki/concepts/`）

必须包含：

* `## Definition` —— 用通俗易懂的一段话定义该概念
* `## How It Works` —— 原理、机制或结构
* `## Key Parameters` —— 关键变量、维度或影响因素
* `## When To Use` —— 适用场景和使用时机
* `## Risks & Pitfalls` —— 已知风险、常见错误和局限性
* `## Related Concepts` —— 相关 Wiki 页面链接
* `## Sources` —— 支撑本页面的原始资料

---

### 实体页面（`wiki/entities/`）

必须包含：

* `## Overview` —— 实体概述
* `## Characteristics` —— 关键特征、属性和结构
* `## How to Use` —— 使用方法、配置示例、快速开始
* `## Related Entities` —— 相关实体页面链接

---

### 综合分析页面（`wiki/syntheses/`）

必须包含：

* `## Comparison` —— 对比表格或结构化比较
* `## Analysis` —— 跨主题分析与洞察
* `## Recommendations` —— 在何种情况下应优先选择哪种方案
* `## Pages Compared` —— 所涉及页面的链接

## 链接规范（Linking Conventions）

* 使用 Obsidian 风格 Wiki 链接：
  `[[concepts/concept-name]]`
* 始终使用**相对于 wiki 根目录**的路径
* 每个页面必须至少链接到另一个页面（禁止孤立页面）
* 当提到一个已有页面的概念时，必须添加对应链接

## 标签体系（Tagging Taxonomy）

<!-- 自定义：请将下面的占位标签替换成适合你领域的标签。 -->

<!-- 每个类别建议包含 3～8 个标签。 -->

<!-- 例如（烹饪知识库）： -->

<!-- Cuisine：italian、japanese、french、mexican -->

<!-- Technique：braising、fermenting、sous-vide、grilling -->

<!-- Ingredient：protein、vegetable、grain、dairy -->

* **类别 A（Category-A）**

  * `tag-1`
  * `tag-2`
  * `tag-3`

* **类别 B（Category-B）**

  * `tag-4`
  * `tag-5`
  * `tag-6`

* **类别 C（Category-C）**

  * `tag-7`
  * `tag-8`
  * `tag-9`

* **范围（Scope）**

  * `foundational`（基础）
  * `advanced`（高级）
  * `experimental`（实验性）

* **状态（Status）**

  * `well-established`（成熟）
  * `emerging`（新兴）
  * `speculative`（推测）

## 可信度等级（Confidence Levels）

* **high（高）**

  * 已广泛验证
  * 有多个相互印证的来源
  * 具有具体案例支持

* **medium（中）**

  * 有资料支持
  * 但案例有限或仅来自单一来源

* **low（低）**

  * 仅出现一次
  * 属于轶事
  * 或仍具有推测性质

## 工作流程（Workflows）

### Ingest（导入）

当用户输入：

```
ingest [source]
```

或者向 `raw/` 添加文件时：

1. 完整阅读原始资料
2. 创建 `wiki/summaries/<source-slug>.md`，生成完整摘要
3. 识别资料中涉及的所有概念、实体和策略
4. 对于每个概念或实体：

   * 如果不存在，则创建页面
   * 如果已存在，则补充更新信息
5. 在所有相关页面之间建立双向交叉链接
6. 更新 `wiki/index.md`

   * 添加新页面
   * 更新已修改页面的摘要
7. 向 `wiki/log.md` 追加记录：

   * 时间戳
   * 来源名称
   * 新建/更新页面列表
8. 若与已有 Wiki 内容存在冲突，则进行标记

### Query（查询）

当用户提出问题时：

1. 读取 `wiki/index.md` 找到相关页面
2. 阅读这些页面
3. 综合生成答案，并引用对应 Wiki 页面（使用 Wiki 链接）
4. 如果产生值得保留的新洞见：

   * 在 `wiki/syntheses/` 创建综合分析页面
   * 更新索引和日志

### Lint（检查）

当用户输入：

```
lint
```

或：

```
health check
```

时：

1. 读取所有 Wiki 页面
2. 检查：

   * 孤立页面（无入链）
   * 过时内容
   * 页面之间存在矛盾
   * 缺失交叉链接
   * 缺少必要章节
   * 可进一步增强的低可信度页面
3. 自动修复能够修复的问题
4. 报告需要人工判断的问题
5. 建议新的资料来源或值得研究的新主题
6. 更新日志

## 规则（Rules）

* **绝不要修改** `raw/` 中的任何文件
* **每次修改 Wiki 后，必须同步更新** `index.md` 和 `log.md`
* 优先更新已有页面，而不是创建重复页面
* 如果对某项内容存在疑问：

  * 将 `confidence` 设为 `"low"`
  * 明确记录不确定性
* 页面应保持聚焦：

  * 一个页面只讨论一个概念
  * 如果内容过长，应拆分页面
* 使用通俗易懂的英语（或对应语言）：

  * 每次首次出现术语时都要进行解释
* 所有日期统一采用 **ISO 8601** 格式：

  ```
  YYYY-MM-DD
  ```
* 当原始资料包含具体案例时，应保留这些案例，并尽量提供完整细节
