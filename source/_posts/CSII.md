---
title:  ChatBot
comments: true
toc: true
categories:
  - CSII
tags:
  - 专业课
abbrlink: 26753
date: 2022-4-26 15:35:11
mathjax: true
cover:  https://user-images.githubusercontent.com/74918703/155833614-94542aa5-0d30-4691-95d6-9d35a95eaa6a.png

---

## the design of CHatBot

## some resources

**GPT-2 中文文本生成**

**Re-Subject：**

1. **采用此组合为主要的参考内容，其他的为辅**

   https://github.com/yangjianxin1/GPT2-chitchat

   （近期也在开始琢磨这个项目）

   > **使用指南见：**[GPT-2生成式多轮对话入门-----深入理解“用于中文闲聊的GPT2模型”项目_三重极简的博客-CSDN博客_gpt2](https://blog.csdn.net/g534441921/article/details/104312983)

2. https://github.com/thu-coai/CDial-GPT【[论文](GPT.pdf)】

3. https://github.com/Morizeyao/GPT2-Chinese【信度高、体量大 / 相关面广，下辖可选项多】

4. [hughqiu/GPT2-Chinese: Chinese version of GPT2 training code, using BERT or BPE tokenizer. (github.com)](https://github.com/hughqiu/GPT2-Chinese) 【中意】

   > **使用指南见：**[GPT-2中文文本训练及生成_是木子啦~的博客-CSDN博客_gpt2中文生成](https://blog.csdn.net/qq_44543774/article/details/116379722)



**数据集来源：**

1. [大规模中文自然语言处理语料 Large Scale Chinese Corpus for NLP (github.com)](https://github.com/brightmart/nlp_chinese_corpus)
2. [THUCTC: 一个高效的中文文本分类工具 (thunlp.org)](http://thuctc.thunlp.org/#中文文本分类数据集THUCNews)

## the part I'm responsible for

### Front-end development

use HTML,css,js...

there are several ideas I come up with.

- the basic chat frame
- the easy animination
- the loading animination(write a javascript function to generate a "div" when clicked and remove it after calculating)
- show time once a minute
- so on...

it seems this part has been finished

still need to combine it with others code

the animination in the left is inspired by https://copepen.io

### patterned answers

use regular expressions or `import jieba` to deal with the input(jieba can deal the input in a loop)

write a crawler to get the current information like the weather ...

**import some database like wikipedia **

reference: https://github.com/brightmart/nlp_chinese_corpus#readme

keep on learning...

