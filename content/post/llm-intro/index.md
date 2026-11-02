---
title: 'LLM 学习笔记：开篇'
description: '记录学习大语言模型的过程：原理、使用与应用开发'
date: '2026-11-02T22:09:30+08:00'
slug: 'llm-intro'
categories: [LLM]
tags: [LLM, AI, 学习路线]
draft: false
---

## 为什么开这个系列

大语言模型（Large Language Model，LLM）这几年发展得非常快，已经从“新奇玩具”变成了日常写代码、查资料、做产品都离不开的工具。只会用还不够，想把它用好、用到自己的项目里，就需要理解它背后的原理和工程实践。

这个系列用来记录我的学习过程。

## 打算写什么

1. **基本原理**：Token、Transformer、注意力机制、预训练与微调分别是什么。
2. **用好模型**：Prompt 编写技巧、上下文管理、如何减少“幻觉”。
3. **应用开发**：调用模型 API、RAG（检索增强生成）、Agent 与工具调用。
4. **本地部署**：在自己的电脑上运行开源模型，了解量化和推理框架。

## 一个最简单的例子

LLM 的本质是“根据前文预测下一个 Token”。调用一个模型 API 通常长这样（以 Python 为例）：

```python
from openai import OpenAI

client = OpenAI()
response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "你是一个乐于助人的助手。"},
        {"role": "user", "content": "用一句话解释什么是大语言模型。"},
    ],
)
print(response.choices[0].message.content)
```

看起来很简单，但从“能调通”到“做出好用的应用”，中间还有很多值得学的东西。后面的文章会一点点记录下来。
