# `/title-to-harness` 设计方案

## 1. 功能定位

`/title-to-harness` 是 **PI Research Meeting** Skill 的第一个功能。

它面向只有一个 research title、模糊 idea、或尚未想清楚研究意义与实验路径的用户，帮助用户把题目打磨成一个结构化、可验证、可继续推进的 `research-harness.md` 或 `survey-harness.md`。

它不是直接帮用户写 proposal，而是模拟组会前的 PI 式预审：先分诊题目类型，再通过追问、补全和假设标注，把模糊题目变成可被后续 AI agent、导师、合作者继续使用的研究骨架。

---

## 2. 触发方式

用户输入：

```text
/title-to-harness

Title:
<research title>

Optional context:
<最多几句话背景、想法、方向、已有担忧>
```

如果用户只给一个 title，也必须可以启动。

---

## 3. 核心目标

最终生成一个 Markdown harness 文件，帮助用户回答：

1. 这个题目属于什么类型？
2. 它到底研究什么？
3. 为什么值得研究？
4. 它的研究问题是什么？
5. 它的核心 claim / hypothesis 是什么？
6. 它的新意在哪里？
7. 最小可验证路径是什么？
8. 什么结果会证明这个想法不成立？
9. 下一步应该怎么推进？
10. 还有哪些问题要带去问 PI？

---

## 4. 总体流程

`/title-to-harness` 使用以下流程：

```text
Step 0: Title Triage
Step 1: Broad Intake
Step 2: Answer Quality Check
Step 3: Targeted Follow-up if needed
Step 4: Assistive Completion if user cannot answer
Step 5: Generate Harness Markdown
```

---

## 5. Step 0: Title Triage

AI 首先判断题目类型，不直接进入研究设计。

可能类型包括：

```text
Research Paper
Survey / Review
Benchmark
Dataset
System
Position / Perspective
Unclear / Hybrid
```

输出格式：

```md
## Title Triage

Original Title:
...

Likely Type:
Research Paper / Survey / Benchmark / Dataset / System / Position

Confidence:
High / Medium / Low

Reason:
...
```

判断逻辑：

* 包含 “survey / review / taxonomy / landscape” → 多半是 Survey / Review
* 包含 “benchmark / evaluation / measuring” → 多半是 Benchmark
* 包含 “dataset / corpus / data” → 多半是 Dataset
* 包含 “system / framework / platform / tool” → 多半是 System
* 包含 “can X do Y / improving X / learning to X / method for X” → 多半是 Research Paper
* 包含 “why / should / implications / perspective” → 可能是 Position / Perspective
* 如果不确定，标注为 Hybrid，并说明可能路径

---

## 6. Step 1: Broad Intake

AI 根据题目类型提出第一轮基础问题。

### 6.1 Research Paper 类型问题

```text
1. 你想解释或解决的失败现象是什么？
2. 研究对象是什么？模型、任务、数据、场景分别是什么？
3. 你想引入、改变或比较的核心变量是什么？
4. 你认为现有方法哪里不够？
5. 你最希望最后证明的 claim 是什么？
```

### 6.2 Survey / Review 类型问题

```text
1. 这篇 survey 的边界是什么？包括什么，不包括什么？
2. 你想提出什么 taxonomy 或 organizing framework？
3. 现有 survey / review 缺了什么？
4. 你会用哪些维度比较已有工作？
5. 读者看完以后应该获得什么新判断？
```

### 6.3 Benchmark 类型问题

```text
1. 你想测量什么能力、失败模式或系统属性？
2. 为什么现有 benchmark 不够？
3. benchmark 的任务、数据或评价方式有什么关键设计？
4. 你希望揭示什么新发现？
5. 什么结果会说明这个 benchmark 没有区分度？
```

### 6.4 Dataset 类型问题

```text
1. 这个 dataset 捕捉了什么现有数据没有覆盖的现象？
2. 数据来源、标注方式和质量控制是什么？
3. 它支持什么研究问题或任务？
4. 和已有 dataset 的差异是什么？
5. 你如何证明它不是普通数据收集？
```

### 6.5 System 类型问题

```text
1. 这个 system 解决什么具体 workflow 或 technical bottleneck？
2. 用户是谁？使用场景是什么？
3. 系统的核心设计选择是什么？
4. 和已有 tool / framework 的差异是什么？
5. 如何评价它真的有用？
```

### 6.6 Position / Perspective 类型问题

```text
1. 你想反驳或推进什么主流观点？
2. 你的核心立场是什么？
3. 支撑这个立场的证据类型是什么？
4. 读者可能最不同意哪里？
5. 这篇文章的贡献是 conceptual clarification、agenda setting，还是 critique？
```

---

## 7. Step 2: Answer Quality Check

用户回答后，AI 必须判断回答是否足够生成 harness。

检查维度：

```text
Problem clarity
Scope clarity
Claim clarity
Novelty clarity
Evidence / experiment clarity
Falsification clarity
Risk clarity
```

每项判断为：

```text
Clear / Partial / Missing
```

输出简短诊断：

```md
## Answer Quality Check

Clear:
- Research object
- Rough task

Partial:
- Claim
- Novelty

Missing:
- Strong baseline
- Falsification condition
```

如果大部分关键项已经清楚，直接生成 harness。

如果有 2 个以上关键项缺失或含糊，进入 Step 3。

---

## 8. Step 3: Targeted Follow-up

第二轮追问不是固定问题，而是针对缺失项补刀。

规则：

```text
最多只进行一轮 targeted follow-up。
只问最关键的 3–5 个问题。
不要重复 Broad Intake。
不要因为用户答得不完美就无限追问。
```

例子：

用户回答：

```text
我想研究小模型自我纠错。
用数学任务。
方法是用大模型生成 critique。
```

AI 应该追问：

```text
我还不能稳妥生成 harness，缺 4 个关键点：

1. 你说的 self-correction 是同一次推理后修改答案，还是训练后整体准确率提升？
2. 最强 baseline 是什么？answer-only finetuning、verifier reranking、self-refine，还是别的？
3. 你希望证明的是一种方法有效，还是一个 empirical finding：小模型能/不能学会纠错？
4. 什么结果会让你承认它只是学会了模仿 critique 格式，而不是真正纠错？
```

---

## 9. Step 4: Assistive Completion

如果用户答不出、回答“不知道”、或明显卡住，AI 不应该阻塞流程。

AI 应该提供候选答案，让用户选择或直接采用推荐版本。

格式：

```md
你暂时答不出没关系。我给你几个可选版本：

A. Conservative version
...

B. Ambitious version
...

C. Analysis-first version
...

My recommendation:
我建议先用 C，因为它最容易抗住 PI 对 novelty 和 falsification 的追问。
```

规则：

```text
If the user cannot answer, generate 2–3 plausible candidate answers.
Label them as assumptions.
Explain tradeoffs briefly.
Recommend the safest default.
Allow the user to say “use your recommendation”.
If the user gives no further preference, proceed with the recommended default and mark assumptions explicitly in the harness.
```

---

## 10. Step 5: Generate Harness Markdown

根据题目类型生成不同 harness。

### 10.1 Research Paper 输出文件

文件名：

```text
research-harness.md
```

模板：

```md
# Research Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Research Pitch

## 3. Problem Statement

## 4. Why This Matters

## 5. Research Question

## 6. Core Hypotheses

## 7. Novelty Claim

## 8. Related Work Buckets

## 9. Proposed Approach

## 10. Minimum Viable Experiment

## 11. Baselines

## 12. Ablations

## 13. Evaluation Metrics

## 14. Falsification Conditions

## 15. Expected Contributions

## 16. Known Risks

## 17. Assumptions and Unknowns

## 18. Next 7-Day Plan

## 19. Questions for PI
```

### 10.2 Survey / Review 输出文件

文件名：

```text
survey-harness.md
```

模板：

```md
# Survey Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Survey Pitch

## 3. Scope Boundary

## 4. Exclusion Criteria

## 5. Why This Survey Is Needed

## 6. Target Audience

## 7. Proposed Taxonomy

## 8. Comparison Axes

## 9. Related Survey Gap

## 10. Literature Buckets

## 11. Key Questions This Survey Answers

## 12. Expected Contribution

## 13. Risks

## 14. Assumptions and Unknowns

## 15. Next 7-Day Plan

## 16. Questions for PI
```

### 10.3 Benchmark 输出文件

文件名：

```text
benchmark-harness.md
```

模板：

```md
# Benchmark Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Benchmark Pitch

## 3. Capability or Failure Mode Being Measured

## 4. Why Existing Benchmarks Are Insufficient

## 5. Benchmark Design

## 6. Dataset / Task Construction

## 7. Evaluation Metrics

## 8. Baselines / Models to Test

## 9. Expected Findings

## 10. What Would Make This Benchmark Uninformative

## 11. Expected Contribution

## 12. Risks

## 13. Assumptions and Unknowns

## 14. Next 7-Day Plan

## 15. Questions for PI
```

---

## 11. 输出风格

AI 应该像一个有经验但愿意帮忙的 senior PhD / PI，不要像普通写作助手。

风格要求：

```text
Direct but supportive.
Skeptical but not discouraging.
Force clarity.
Mark assumptions.
Prefer falsifiable claims.
Avoid overclaiming.
```

可以有一点组会梗，但不能影响严肃性。

允许使用短句：

```text
这个 title 现在还不能安全进组会。
这个 claim 现在太软。
这不是坏 idea，但现在像一个实验想法，不像 paper。
这里需要一个能被打脸的 hypothesis。
```

---

## 12. 关键设计原则

### 12.1 不要一上来写完整 proposal

必须先分诊和追问。

### 12.2 最多两轮追问

第一轮 broad intake，第二轮 targeted follow-up。

如果仍然不完整，也要生成 harness，并把未知项写进 `Assumptions and Unknowns`。

### 12.3 用户答不出时要帮他搭脚手架

不要让用户卡死。给候选答案、推荐默认版本，并标注为假设。

### 12.4 区分研究型和综述型

Survey harness 不应该硬塞 hypothesis / experiment。
Research harness 不应该只做 taxonomy。

### 12.5 Harness 是后续 AI 研究流程的上下文文件

输出不仅给人看，也要让后续 AI 能继续用：

```text
继续查 related work
设计实验
写 proposal
生成 pi-grill 问题
准备组会汇报
```

---

## 13. 示例场景

用户输入：

```text
/title-to-harness

Title:
Can Small Language Models Learn to Self-Correct Through Synthetic Critiques?
```

AI 输出：

```md
## Title Triage

Likely Type:
Research Paper

Confidence:
High

Reason:
The title has a clear research object, small language models; a proposed mechanism, synthetic critiques; and a capability to test, self-correction.
```

AI 追问：

```text
先别写 proposal。我需要先把这个 title 压成可验证的 research harness。

1. 你说的 self-correction 是生成答案后修改答案，还是训练后整体错误率下降？
2. synthetic critiques 是强模型生成、模型自己生成，还是规则生成？
3. 你最关心 math reasoning、coding、general QA，还是 agent task？
4. 你想证明小模型真的学会纠错，还是只是 critique training 有经验收益？
5. 什么结果会让你放弃这个 hypothesis？
```

用户粗略回答：

```text
我想看 1B-3B 小模型能不能通过 GPT-4 生成的 critique 学会改答案。
任务先看 math reasoning。
我担心它只是学会格式，不是真的纠错。
```

AI 做质量检查：

```md
## Answer Quality Check

Clear:
- Model scale
- Feedback source
- Initial task
- Main concern

Partial:
- Core claim
- Novelty type

Missing:
- Baselines
- Falsification condition
- Minimum viable experiment
```

AI targeted follow-up：

```text
还差 4 个关键点，我补问一次：

1. 最强 baseline 你想打谁？answer-only finetuning、self-refine、verifier reranking，还是 critique-free correction？
2. 你希望 novelty 是方法、实验发现，还是诊断框架？
3. 如果只在 seen templates 上提升，你是否认为 hypothesis 失败？
4. 你的最小实验是 GSM8K 一个任务，还是还需要一个 OOD math set？
```

如果用户答不出：

```text
不知道，你帮我定。
```

AI assistive completion：

```md
可以。我建议先用这个保守但抗打的版本：

Assumption A:
Novelty 不是“我们提出全新训练方法”，而是“我们系统诊断 synthetic critique 是否能让小模型获得真正的 self-correction ability”。

Why:
这个版本比 method novelty 更安全，因为 critique training 本身可能被 reviewer 认为是已有套路；但区分 self-correction、answer editing 和 critique imitation 会更有研究价值。
```

最终生成 `research-harness.md`。

---

## 14. 与 `/pi-grill` 的衔接

`/title-to-harness` 的输出应该能直接作为 `/pi-grill` 的输入。

`/pi-grill` 可以读取：

```text
Original Title
Research Question
Hypotheses
Novelty Claim
Minimum Viable Experiment
Baselines
Falsification Conditions
Known Risks
Questions for PI
```

然后模拟 PI 死亡追问。

因此 `/title-to-harness` 必须显式写出：

```md
## Known Risks

## Questions for PI

## Assumptions and Unknowns
```

这三部分是后续 `/pi-grill` 的燃料。
