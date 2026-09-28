---
name: make-knowledge-cards
description: Convert pasted article text or local Markdown/TXT files into 5-8 knowledge cards for study and review. Each card contains a title, core knowledge point, concise explanation, and an example or self-test question. Use this skill when the user asks to "make knowledge cards", "turn this article into cards", "extract knowledge points", "create study cards", or provides an article/file and wants condensed study material. Supports plain text input and local .md/.txt files only; does not support web scraping, PDF, Anki export, or graphical interfaces.
---

# Make Knowledge Cards

## Overview

Transform an article (pasted text or a local Markdown/TXT file) into a set of 5-8 self-contained knowledge cards that capture the genuinely important knowledge for later study and review.

## Input Handling

Accept input in one of two forms:

1. **Pasted text** — article content included directly in the conversation.
2. **Local file path** — a `.md` or `.txt` file. Read the file first; if the path does not exist, the extension is not `.md`/`.txt`, or the file cannot be read, tell the user and ask them to paste the content instead.

If the user provides neither text nor a file path, ask for the article. Do not generate cards without source material.

If the user asks to fetch a URL, process a PDF, export to Anki, or show a graphical interface: state clearly that these are not supported yet, then offer the supported alternative (paste the text or provide a local Markdown/TXT file).

## Workflow

1. **Read the full source material** before writing anything. Understand the article's topic, structure, and main claims.
2. **Extract knowledge points.** Identify concepts, facts, principles, methods, or arguments that are genuinely important to understanding the article. Prioritize:
   - Central concepts and definitions the article spends significant space on
   - Causal relationships, principles, or rules the article explains
   - Methods, steps, or decision criteria the article teaches
   - Important distinctions or comparisons the article makes
3. **Deduplicate and merge.** If the same idea appears multiple times (in the intro, body, and conclusion), keep one card covering it, using the fullest expression of the idea.
4. **Write the cards** in the output format below. Generate 5-8 cards.
5. **Review against the rules** before presenting (see Quality Rules), then output the cards and a one-line summary stating the total number of cards.

## Card Format

Output all cards in Markdown using exactly this structure:

```markdown
## 卡片 1：<标题>

**核心知识**：<一句话陈述这个知识点>

**解释**：<2-4 句话，用自己的话讲清楚这个知识点>

**例子 / 自测**：<一个来自原文的具体例子，或一个针对该知识点的自测问题>
```

Field rules:

- **标题**: short (preferably under 15 characters), names the single knowledge point.
- **核心知识**: one sentence stating the point as a self-contained fact — readable without the original article.
- **解释**: 2-4 sentences in plain language. Restate what the article actually says; do not add outside knowledge.
- **例子 / 自测**: prefer a concrete example from the source. If the article has no usable example, write a self-test question whose answer is found in the source. Label which one it is (e.g. `例子：...` or `自测：...`).

## Quality Rules

These rules override card-count targets. Check every card against them:

- **One card, one knowledge point.** If a card needs "and also..." to explain, split it into two cards or narrow it to the more important point.
- **No fabrication.** Every statement in 核心知识 and 解释 must be traceable to the source. Never invent examples, numbers, or claims the article does not contain. If uncertain whether something is in the source, omit it.
- **Importance first.** Drop cards about minor details, boilerplate, or transitions just to reach 5 cards. Conversely, if the article is rich and supports more than 8 cards, keep the 8 most important.
- **Insufficient information = fewer cards.** If the source genuinely supports fewer than 5 distinct important points (e.g. a very short note or a single-idea snippet), produce only as many cards as warranted (even if that is just 1-2) and briefly tell the user the source was too thin to produce more. Never pad with cards that restate the same point in different words.
- **Deduplicate.** Before finalizing, scan all cards: if two cards overlap significantly, merge them into one.
- **Order by logic.** Arrange cards in the article's logical order; if the article has no clear order, arrange by importance.
- **Self-contained.** Assume the reader has no access to the original article. Replace pronouns and vague references ("这个方法", "如上文所述") with explicit names.
- **Preserve the source language.** Write cards in the same language as the source article.
- **Preserve important specifics.** Keep numbers, names, and conditions exactly as the source states them.

## Output Example

```markdown
## 卡片 1：飞轮效应

**核心知识**：亚马逊的飞轮效应指低价带来更多客户，更多客户带来更大规模，更大规模进一步降低成本，形成自我强化的增长循环。

**解释**：贝佐斯在 2001 年提出了飞轮模型。它把增长描述为一个闭环：规模降低成本 → 成本降低价格 → 低价吸引客户 → 客户扩大规模。各环节互为因果，一旦转动起来会持续加速，无需外部推动。

**例子**：原文提到亚马逊坚持长期低价、牺牲短期利润，正是为了让飞轮先转起来。

---

（后续卡片依次类推）

**共 6 张卡片。**
```
