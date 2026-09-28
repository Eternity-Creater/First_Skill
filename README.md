# make-knowledge-cards

一个可开源的 CodeBuddy Skill：把用户粘贴的文章或本地 Markdown/TXT 文件转换成 **5～8 张知识卡片**，用于学习和复习。

## 介绍

面对一篇长文章，最费力的不是读完，而是事后回忆"它到底讲了什么"。这个 Skill 让 AI 按照固定的质量标准替你完成提炼：先通读全文，识别真正重要的知识点，去重后为每个知识点生成一张独立的知识卡片。

核心设计原则：

- **一卡一点**：一张卡片只讲一个知识点，绝不混杂
- **只提炼、不编造**：所有内容必须能追溯到原文，不添加原文没有的信息
- **宁缺毋滥**：原文信息量不足时减少卡片数量，绝不强行凑数
- **自包含**：卡片离开原文也能独立读懂

暂不支持：网页抓取、PDF、Anki 导出、图形界面。

## 项目结构

```
skills/make-knowledge-cards/
├── SKILL.md          # Skill 入口：工作流程、卡片格式、质量规则
└── agents/
    └── openai.yaml   # 界面配置：显示名称、简介、默认调用语

test-samples/         # 开发时的测试样本与生成示例
```

## 使用方法

1. 将 `skills/make-knowledge-cards` 安装为项目级或用户级 Skill。
2. 触发方式（任选其一）：
   - 直接粘贴文章内容，说"把这篇文章转换成知识卡片"
   - 提供本地 `.md` / `.txt` 文件路径，如"用知识卡片总结 `notes.md`"
   - 使用默认调用语：`请使用 $make-knowledge-cards 把这篇文章转换成知识卡片：`

## 输入输出示例

**输入**（粘贴文章或提供文件，节选自 `test-samples/git-rebase-merge.md`）：

> `git rebase main` 会把你分支上的提交逐个"摘下来"，放到 main 的最新提交之后重新提交，生成一串全新的 commit hash。历史变成一条直线，干净整洁……

**输出**：

> ## 卡片 2：Rebase 的本质
>
> **核心知识**：`git rebase` 把分支上的提交逐个"摘下来"，放到目标分支最新提交之后重新提交，使历史变成一条直线。
>
> **解释**：rebase 之后的历史干净整洁。但新提交与原提交内容相同、commit hash 却是全新的，等于改写了历史。
>
> **自测**：rebase 之后，分支上原有提交的 hash 还和原来一样吗？

完整示例见 `test-samples/output-git-rebase-merge.md`（技术教程，6 张卡片）、`test-samples/output-slow-thinking.md`（观点文章，5 张卡片）、`test-samples/output-tiny-note.md`（超短笔记，1 张卡片——演示"不强行凑数"规则）。
