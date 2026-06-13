# `/pi-grill` 设计方案

## 1. 功能定位

`/pi-grill` 是 **PI Research Meeting** Skill 的第二个功能。

它接收 `/title-to-harness` 生成的 harness markdown 作为必需输入，并可额外接收 abstract、proposal、draft、experiment plan 或 PDF。它模拟真实组会里 PI 对研究设计的少量但致命追问，帮助用户发现 claim、novelty、experiment、baseline、falsification 中最危险的问题。

它不是普通 reviewer checklist，而是 **concise PI-style research stress test**：

```text
少问，狠问，问到会改变研究方向的问题。
```

---

## 2. 触发方式

用户输入：

```text
/pi-grill

Required:
<research-harness.md / survey-harness.md / benchmark-harness.md>

Optional:
<abstract / proposal / draft / experiment plan / paper PDF>
```

支持输入形式：

```text
- 粘贴 markdown 内容
- 上传 .md / .txt
- 上传 .pdf
- 粘贴 abstract 或 draft 片段
```

---

## 3. 输入门槛

### 3.1 必需输入

`/pi-grill` 必须要求一个由 `/title-to-harness` 生成的 harness markdown 文件。

可接受文件名包括：

```text
research-harness.md
survey-harness.md
benchmark-harness.md
dataset-harness.md
system-harness.md
position-harness.md
```

如果用户没有提供 harness，必须停止并要求用户先提供：

```text
我需要先看到 /title-to-harness 生成的 harness.md。
你可以同时贴 abstract、draft 或 PDF，但 harness 是必需输入。
```

### 3.2 可选输入

用户可以额外提供：

```text
- Abstract
- Proposal
- Paper draft
- Experiment plan
- Related work notes
- Manuscript PDF
- Slides / lab meeting notes
```

这些材料只作为 supporting evidence，不替代 harness。

---

## 4. Source of Truth 规则

harness 是 source of truth。

AI 必须优先从 harness 读取：

```text
Original Title
Paper Type
Research Question
Core Hypotheses
Novelty Claim
Minimum Viable Experiment
Baselines
Ablations
Evaluation Metrics
Falsification Conditions
Known Risks
Assumptions and Unknowns
Questions for PI
```

如果 optional draft / abstract / PDF 和 harness 不一致，AI 应该指出：

```md
## Consistency Warning

The draft seems to claim X, but the harness only supports Y.
This may create an overclaim risk.
```

---

## 5. 总体流程

```text
Step 0: Input Gate
Step 1: Material Parsing
Step 2: Meeting Diagnosis
Step 3: Round 1 — PI First Reaction
Step 4: User Answer Stress Test
Step 5: Round 2 — Targeted Follow-up if needed
Step 6: Final Repair Synthesis
Step 7: Generate pi-grill-report.md
```

---

## 6. Step 0: Input Gate

AI 首先检查是否有 harness。

如果没有 harness：

```text
Stop. Ask for harness.
Do not run PI grill from abstract alone.
```

如果有 harness：

```text
Proceed.
```

如果同时有 optional material：

```text
Use it to sharpen questions and detect mismatch.
Do not let it override the harness unless the user explicitly says the harness is outdated.
```

---

## 7. Step 1: Material Parsing

AI 提取材料中的核心字段。

输出内部诊断即可，不需要太长。

```md
## Parsed Inputs

Harness Type:
Research Harness / Survey Harness / Benchmark Harness / ...

Optional Materials:
- Abstract: yes/no
- Draft: yes/no
- PDF: yes/no
- Experiment plan: yes/no

Key Claim:
...

Key Risk:
...

Key Uncertainty:
...
```

如果 PDF 或 draft 中信息不足，标注：

```text
Optional materials were too thin to materially change the grill.
```

---

## 8. Step 2: Meeting Diagnosis

AI 判断当前材料处于什么阶段：

```text
Harness stage
Proposal stage
Experiment design stage
Preliminary results stage
Draft paper stage
Revision / rebuttal stage
```

然后给出 PI 可能最关注的一个主问题。

输出格式：

```md
## Meeting Diagnosis

Stage:
Proposal stage

PI likely focus:
Whether the proposed experiment can actually support the stated claim.

Main danger:
The current claim says “self-correction ability,” but the experiment may only show “critique-conditioned answer editing.”
```

原则：

```text
只诊断最核心危险，不列 checklist。
```

---

## 9. Step 3: Round 1 — PI First Reaction

第一轮只问 2–3 个问题。

硬规则：

```text
Ask no more than 3 questions.
Prefer 2 questions if they are enough.
Each question must be capable of killing, reshaping, or significantly narrowing the project.
Do not ask generic checklist questions.
```

问题格式：

```md
## Round 1: PI First Reaction

### Q1. <fatal question>

Why this hurts:
...

What a strong answer needs:
...

### Q2. <fatal question>

Why this hurts:
...

What a strong answer needs:
...
```

问题优先级：

```text
1. Claim 是否过大
2. Novelty 是否成立
3. Experiment 是否能证明 claim
4. Baseline 是否会直接杀死 idea
5. Falsification 是否明确
6. Survey / benchmark / system 类型是否有真实贡献
```

---

## 10. 不同 Harness 类型的 PI 问法

### 10.1 Research Harness

常见致命问题：

```text
1. 你的实验到底证明了核心 claim，还是只证明了一个更弱的 proxy？
2. 最强 baseline 如果赢了，你这篇 paper 还剩什么？
3. 你的 novelty 是 method、empirical finding、analysis，还是只是工程组合？
4. 有没有 alternative explanation 可以解释所有正结果？
5. 什么结果会让你承认 hypothesis 错了？
```

### 10.2 Survey Harness

常见致命问题：

```text
1. 你的 taxonomy 为什么不是普通分类，而是能改变读者理解的框架？
2. 和最近已有 survey 相比，你的新判断是什么？
3. 你的 comparison axes 是否能产生 insight，而不只是表格？
4. 读者看完后会改变什么 belief？
5. 这篇 survey 最容易被批评成文献堆砌，你怎么避免？
```

### 10.3 Benchmark Harness

常见致命问题：

```text
1. 这个 benchmark 测到的是目标能力，还是某种 dataset artifact？
2. 如果所有模型分数都差不多，这个 benchmark 还有价值吗？
3. 为什么现有 benchmark 不够？
4. 你如何证明它有区分度和诊断性？
5. 它会不会很快被 benchmark gaming？
```

### 10.4 Dataset Harness

常见致命问题：

```text
1. 这个 dataset 捕捉了什么现有数据没有的现象？
2. 为什么这不是普通数据收集？
3. 数据质量、偏差和标注一致性怎么保证？
4. 它能支持什么新的研究问题？
5. 如果模型提升来自数据规模而不是数据设计，你怎么证明贡献？
```

### 10.5 System Harness

常见致命问题：

```text
1. 这个 system 解决的是研究问题，还是只是工程实现？
2. 你的 evaluation 如何证明它真的改善 workflow？
3. 和已有 tool / framework 的核心差异是什么？
4. 用户为什么需要它？
5. 它的设计 insight 能否被抽象成 paper contribution？
```

### 10.6 Position / Perspective Harness

常见致命问题：

```text
1. 你的核心立场是否足够非平凡？
2. 你反驳或修正了哪个主流观点？
3. 证据是否足以支撑这个立场？
4. 最强反对意见是什么？
5. 读者看完后应该改变什么判断？
```

---

## 11. Step 4: User Answer Stress Test

用户回答 Round 1 后，AI 评估每个回答。

评分标签：

```text
Strong
Okay
Hand-wavy
Dangerous
Missing
```

输出格式：

```md
## Answer Stress Test

### Q1 Verdict:
Hand-wavy

Why:
你解释了方法，但没有解释为什么该方法能证明 “self-correction ability”。

Stronger answer should:
- Narrow the claim
- Name the alternative explanation
- Add one diagnostic experiment

Suggested repair:
把 claim 从 “model learns self-correction” 改成 “model improves critique-conditioned answer revision, with additional tests for genuine error diagnosis.”
```

规则：

```text
Be direct.
Do not be mean.
Do not over-explain.
For every weak answer, provide a repair path.
```

---

## 12. Step 5: Round 2 — Targeted Follow-up

第二轮不是必需的。

触发条件：

```text
Run Round 2 only if:
- The user’s answer is Hand-wavy / Dangerous / Missing on a core issue
- One more question could materially improve the research plan
```

如果第一轮回答足够好：

```text
Skip Round 2 and generate report.
```

第二轮问题数量：

```text
Ask 1–2 questions only.
Never ask more than 2 questions in Round 2.
```

格式：

```md
## Round 2: Targeted Follow-up

### Q1. <targeted question>

This is the key question because:
...

### Q2. <optional second question>

This is the key question because:
...
```

示例：

```text
你现在把 self-correction 定义成 “answer revision improves after critique”。
那我追问一个关键点：你如何区分真正 error diagnosis 和学会了 critique-style rewriting？
```

---

## 13. Step 6: Final Repair Synthesis

无论用户第二轮答得如何，AI 最终必须收敛到修复方案。

生成：

```text
PI’s main concern
Weakest claim
Safer claim
Evidence needed
Experiments / analyses to add
What to fix before next meeting
Next 7-day plan
Context for future AI work
```

核心原则：

```text
先打，再救。
先指出致命问题，再给能推进的修复路径。
```

---

## 14. Step 7: Generate `pi-grill-report.md`

最终输出文件名：

```text
pi-grill-report.md
```

模板：

```md
# PI Grill Report

## 0. Input Type and Stage

## 1. PI’s Main Concern

## 2. The Questions That Matter

### Q1. ...
- Why this matters:
- Current answer quality:
- Stronger answer:
- Evidence needed:

### Q2. ...
- Why this matters:
- Current answer quality:
- Stronger answer:
- Evidence needed:

### Q3. ...
- Why this matters:
- Current answer quality:
- Stronger answer:
- Evidence needed:

## 3. Claim Risk Assessment

## 4. Weakest Claim

## 5. Safer Claim

## 6. Novelty Risk

## 7. Experiment / Evidence Risk

## 8. Baseline / Comparison Gaps

## 9. Alternative Explanations

## 10. What to Fix Before Next Meeting

## 11. Next 7-Day Plan

## 12. Updated Questions for PI

## 13. Context for Future AI Work
```

---

## 15. 输出风格

`/pi-grill` 应该像真实 PI 或 senior advisor：

```text
Concise.
Skeptical.
High signal.
No checklist dumping.
No fake encouragement.
No endless questions.
```

允许使用风格化短句：

```text
这个 claim 现在撑不住。
这个实验只能证明弱版本。
这不是坏 idea，但现在不是你以为的那篇 paper。
如果 baseline 赢了，你需要知道这篇文章还剩什么。
这个问题值得做，但你现在讲得太大。
```

但必须随后给修复方案：

```text
更安全的说法是...
你下一步只需要补一个诊断实验...
这可以从 method paper 改成 empirical analysis paper...
```

---

## 16. 关键设计原则

### 16.1 Harness 必需

`/pi-grill` 不从零开始 brainstorm。它必须基于 `/title-to-harness` 生成的 harness。

### 16.2 Optional materials 只做增强

abstract、draft、PDF 用来检查一致性、发现 overclaim、补充实验细节。

### 16.3 每轮问题必须少

真实导师不会问 20 个 checklist 问题。每轮只问最要命的几个。

```text
Round 1: 2–3 questions
Round 2: 1–2 questions
```

### 16.4 问题必须改变研究

每个问题都应该满足至少一个条件：

```text
Can kill the project
Can narrow the claim
Can change experiment design
Can expose novelty failure
Can reveal unsupported causal claim
Can force a safer contribution
```

### 16.5 不要只批评

必须形成 repair report。

### 16.6 最终报告要能继续喂给 AI

`pi-grill-report.md` 应能作为后续 AI research agent 的上下文，用于：

```text
改 harness
补实验
写 proposal
改 abstract
设计 rebuttal
准备下一次组会
```

---

## 17. 示例场景

用户输入：

```text
/pi-grill

Attached:
research-harness.md

Optional abstract:
We investigate whether small language models can learn to self-correct through synthetic critiques generated by stronger models...
```

AI 解析：

```md
## Meeting Diagnosis

Stage:
Proposal stage

PI likely focus:
Whether the proposed experiment can support the phrase “learn to self-correct.”

Main danger:
The current setup may only show critique-conditioned answer editing, not genuine self-correction ability.
```

Round 1：

```md
## Round 1: PI First Reaction

### Q1. 你怎么证明小模型学会的是 self-correction，而不是学会了模仿 GPT-4 critique 后改写答案？

Why this hurts:
如果无法区分这两者，你的核心 claim 会从 “self-correction ability” 降级成 “supervised answer editing works.”

What a strong answer needs:
一个诊断实验，例如 unseen error types、critique removal、wrong critique robustness，或 error localization accuracy。

### Q2. 最强 baseline 如果是 answer-only finetuning 或 verifier reranking，它们赢了以后你这篇 paper 还剩什么？

Why this hurts:
如果更简单 baseline 达到同样效果，critique training 的贡献就不成立。

What a strong answer needs:
说明你的贡献可以转向 analysis：什么时候 critique 有用、什么时候只是增加监督信号。
```

用户回答很粗：

```text
我觉得可以做 OOD math，然后和 answer-only finetuning 比。
```

AI stress test：

```md
## Answer Stress Test

### Q1 Verdict:
Okay but incomplete.

Why:
OOD math 能测试泛化，但仍不足以证明 error diagnosis。模型可能只是学会更好的 answer editing。

Suggested repair:
加入 error localization 或 corrupted critique robustness，测试模型是否真的利用 critique 中的错误信息。

### Q2 Verdict:
Okay.

Why:
answer-only finetuning 是合理 baseline，但还需要说明如果它赢了，论文如何转成 negative / diagnostic finding。
```

Round 2：

```md
## Round 2: Targeted Follow-up

### Q1. 如果 answer-only finetuning 和 critique training 差不多，你愿不愿意把论文改成 “synthetic critiques do not reliably teach self-correction”?

This is the key question because:
这决定你的 paper 是 method paper，还是 empirical analysis / negative result paper。
```

最终报告：

```md
# PI Grill Report

## 1. PI’s Main Concern
The project currently overclaims self-correction ability relative to the proposed evidence.

## 2. The Questions That Matter
...

## 5. Safer Claim
Synthetic critique training can improve critique-conditioned revision in small language models, but genuine self-correction requires additional evidence from error localization, OOD transfer, and robustness to misleading critiques.

## 10. What to Fix Before Next Meeting
1. Define self-correction operationally.
2. Add answer-only finetuning and verifier reranking baselines.
3. Add one diagnostic experiment separating critique imitation from error diagnosis.

## 11. Next 7-Day Plan
Day 1: finalize operational definition.
Day 2–3: implement answer-only baseline.
Day 4–5: create corrupted critique diagnostic set.
Day 6: run small-scale pilot.
Day 7: update harness and abstract.
```

---

## 18. 与 `/title-to-harness` 的关系

```text
/title-to-harness
把模糊 title 变成 harness。

/pi-grill
拿 harness 去见 PI，被少量但致命的问题拷打，然后生成 repair report。
```

`/pi-grill` 的输出可以反向更新 harness：

```text
research-harness.md + pi-grill-report.md
→ revised-research-harness.md
```

但默认不自动重写 harness，除非用户明确要求。
