---
title: 智能体工作流1：论文 Camera-Ready 版本生成
date: 2026-05-02 04:51:33
updated: 2026-05-02 04:51:33
categories:
  - AI智能体工作流
tags:
  - AI Agent
  - Camera Ready
  - LaTeX
  - Springer LNCS
  - 论文写作
keywords:
  - 智能体工作流
  - Camera Ready
  - LaTeX
  - Springer LNCS
  - Reviewer Comments
  - AI Agent
description: 记录一次使用 AI 智能体协助完成论文 camera-ready 准备的完整流程，包括 LaTeX 源码整理、PDF 编译验证、Springer LNCS 检查、reviewer comments 修改、v2 打包与辅助文档生成。
---

## 前言（本文内容由AI生成）

这篇文章记录一次使用 AI 智能体协助完成论文 camera-ready 版本筹备的完整过程。任务目标不是简单地让智能体“给建议”，而是让它直接参与本地文件处理：读取提交要求、整理 LaTeX 工程、修复编译问题、生成 PDF 和源码压缩包、检查 Springer 表格、根据 reviewer comments 规划并修改论文，最后输出一组可审阅、可追溯的辅助文档。

本次工作的原始目录位于：

```text
D:\研究生\科研\论文\camera-ready
```

在 WSL 环境中的路径为：

```text
/mnt/d/研究生/科研/论文/camera-ready
```

最终形成了两个主要版本：

- `v1/`：整理后的第一版 camera-ready LaTeX、PDF 和源码 zip；
- `v2/`：基于 reviewer comments 修改后的版本，以及实际改动记录、段落级前后对比、未完全处理项报告等辅助材料。

## 使用的 Skill

本次主要使用的智能体 Skill 是：

```text
latex-camera-ready-packaging
```

这个 Skill 的作用是指导智能体完成 LaTeX camera-ready / Springer LNCS / EasyChair 相关工作，包括：

- 检查提交要求和源码包；
- 在安全目录中解压和整理 LaTeX 工程；
- 识别主 `.tex` 文件、图片、BibTeX、class/style 依赖；
- 编译并修复阻塞性 LaTeX/BibTeX 错误；
- 验证 PDF 页数、参考文献起始页、A4/Letter 页面大小；
- 清理不应上传的模板文件和构建产物；
- 生成最终 PDF 和 source zip；
- 检查 Springer copyright form；
- 基于 reviewer comments 生成修改计划；
- 实际修改 v2 论文，并记录 actual changes、before/after、remaining gaps。

它不是某一次任务临时生成的总结文档，而是一个可复用的工作流规范。智能体会根据这个 Skill 中的步骤执行具体操作。

## 整体工作流概览

这次 camera-ready 准备过程可以拆成 8 个阶段：

```text
1. 读取 camera-ready 要求
2. 整理和验证 v1 LaTeX/PDF/zip
3. 拆分 LaTeX 章节
4. 检查 Springer copyright form
5. 分析 reviewer comments 并生成 review_action_plan.md
6. 基于 review_action_plan.md 实际修改 v2
7. 编译、验证并打包 v2
8. 生成审阅辅助文档
```

下面按阶段记录每一步的输入、处理方式和产出。

## 阶段一：读取 camera-ready 要求

首先，智能体读取了目录中的 `camera-ready.md`，确认 camera-ready 阶段需要提交的材料。

主要确认项包括：

- LaTeX source 或 RTF 源文件；
- final PDF；
- reviewer comments；
- Springer copyright form；
- EasyChair 上传材料。

这一阶段的价值在于先确认“最终要交什么”，避免后续只生成 PDF，却遗漏源码压缩包或版权表格。

## 阶段二：整理和验证 v1 LaTeX/PDF/zip

用户提供的原始 LaTeX 包中，主文件仍使用模板式命名，例如 `samplepaper.tex`。智能体首先将源码解压到工作目录，再识别真正用于编译的主文件、图片、BibTeX 文件和 LNCS 模板依赖。

处理内容包括：

1. 解压原始 LaTeX zip；
2. 识别主文件 `samplepaper.tex`；
3. 检查依赖文件：
   - `llncs.cls`；
   - `splncs04.bst`；
   - `references.bib`；
   - 论文中实际使用的 figures；
4. 安装缺失的 LaTeX 编译依赖；
5. 修复 `references.bib` 中重复 BibTeX entry；
6. 将主文件改名为 `main.tex`；
7. 删除模板说明文件、未使用图片和构建产物；
8. 生成 v1 PDF；
9. 生成 v1 LaTeX source zip；
10. 从干净目录重新编译验证。

这一阶段的主要输出为：

```text
v1/latex/
v1/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_v1.pdf
v1/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_latex_v1.zip
v1/changes_v1.md
```

验证结果显示：

```text
Pages: 18
References: page 17
正文页数: 16 pages excluding bibliography
BUILD_OK
```

也就是说，PDF 总页数为 18 页，参考文献从第 17 页开始，正文部分控制在 16 页以内，符合 “16 pages excluding bibliography” 的限制。

## 阶段三：拆分 LaTeX 章节

为了便于后续根据 reviewer comments 逐章修改，智能体将原本集中在一个主文件中的论文内容拆分成多个章节文件。

处理后的结构大致为：

```text
v1/latex/main.tex
v1/latex/sections/01_introduction.tex
v1/latex/sections/02_running_example.tex
v1/latex/sections/03_formal_definitions.tex
v1/latex/sections/04_framework.tex
v1/latex/sections/05_experimental_evaluation.tex
v1/latex/sections/06_related_work.tex
v1/latex/sections/07_conclusion.tex
```

这样做的好处是后续修改更可控。比如 reviewer 提到 Running Example、Formal Definitions 或 Algorithm 的问题时，可以直接定位到对应章节文件，而不是在一个很长的 `main.tex` 中反复查找。

拆分后，智能体重新编译 PDF 并刷新 zip，避免因为结构调整导致最终上传包和 PDF 不一致。

## 阶段四：Springer copyright form 处理

除论文源码和 PDF 外，camera-ready 还涉及 Springer copyright form。智能体对用户提供或下载的表格进行了检查。

检查内容包括：

- 文件是否可以正常打开；
- `.docx` 和 `.pdf` 是否匹配；
- PDF 页数是否正常；
- 签名区域是否存在明显问题；
- `Volume Editor(s)`、会议名称等字段是否需要进一步确认。

这一步没有简单地把文件当作“已完成”，而是提醒了不确定字段。例如：Program Co-Chairs 不一定等同于 Volume Editors，因此相关字段最好以会议方或 Springer 通知为准。

## 阶段五：分析 reviewer comments 并生成修改计划

当用户提供 `Review.md` 后，智能体没有直接修改论文，而是先读取 reviewer comments 和当前 LaTeX 章节，生成了一个修改计划：

```text
v2/review_action_plan.md
```

这个计划按照 reviewer comments 逐条分析，包含：

- reviewer 提出的具体问题；
- 当前论文中对应的位置；
- 可以在不新增实验的前提下进行的修改；
- 哪些内容不能凭空补充；
- 哪些要求只能写入 limitation、threats to validity 或 future work。

本次用户明确要求“不进行额外实验分析”，因此智能体在计划中遵守了一个重要原则：

> 不虚构实验结果，不编造 success rate、validity rate，也不声称完成了没有实际运行的多模型或 SOTA 对比实验。

这个原则对学术论文修改非常关键。对于 reviewer 希望看到但当前无法新增实验支持的内容，正确处理方式是澄清方法、补充威胁、说明局限，而不是制造不存在的数据。

## 阶段六：基于 review_action_plan.md 实际修改 v2

在生成修改计划后，智能体复制 v1 LaTeX 工程作为 v2 工作区：

```text
v2/latex/
```

然后按照修改计划逐章编辑。

### Running Example

在 Running Example 中，主要补充和修正了：

- Triplex outputs 在 oracle checking / fault detection 中的作用；
- A1–A4 是 LLM 生成后经过检查和 normalization 的 actions；
- RM-003、RM-004 的 scenario coverage；
- 若干标点和空格问题，例如 `.This`、`.Specifically` 这类排版错误。

### Formal Definitions

在 Formal Definitions 中，补充了多个符号定义：

- `Σ`；
- `σ`；
- `σ_S`；
- `ρ`；
- `I_T`；
- STL 中的 `G` operator。

同时还增加了 Triplex A3 formal action tuple 示例，并解释 scenario occurrence 是否允许 gap。当前实现采用 contiguous intervals，因此文中也进行了相应说明。

### Framework / Algorithms

Framework 章节是本次修改的重点之一。智能体补充了：

- LLM output schema 检查；
- signal 检查；
- STL parser 检查；
- regeneration / normalization 机制；
- `x1, x2, x3` 是测试生成阶段的搜索变量；
- Algorithm 1 的重复行和模糊表达修正；
- Algorithm 3 的 candidate generation、input bounds、candidate pool、maximum iterations 和 termination condition。

此外，还增加了 `Compatible` / compatibility check 的解释，用来回应 reviewer 对随机组合 scenario 是否可能 meaningless 的担忧。

### Experimental Evaluation

实验章节中主要完成了：

- 修复 `DeepSeek [7]` 这类 citation 格式问题；
- 修复 citation after period 问题；
- 修复 Table 2 caption；
- 补充 RQ3 fault seeding protocol；
- 解释 NN 模型中 FRR 较低以及 baseline 为 0 的原因；
- 扩展 Threats to Validity。

Threats 中特别补充了以下限制：

- 未系统比较多个 LLM；
- action extraction completeness 没有形式化保证；
- STL constraint generation success rate 未系统报告；
- scenario validity rate 未系统报告；
- 未覆盖所有 SOTA Simulink testing 方法；
- ordered action sequences 对复杂 branching / concurrent requirements 的表达能力有限。

这些内容不是“补实验”，而是把方法边界说清楚。

### Related Work

Related Work 部分扩展了与以下方向的对比：

- Simulink testing；
- requirement-based testing；
- scenario-based testing；
- LLM-based testing。

同时也明确说明，本文没有声称已经全面比较所有 SOTA 方法，而是强调本文方法与 random-search、constraint-solving、falsification、direct LLM test generation 的区别。

### Conclusion and Future Work

Conclusion 中补充了 future work，包括：

- 多 LLM 对比；
- action completeness；
- scenario validity；
- branching / concurrent scenario graphs；
- 更广泛地比较 Simulink falsification 和 model-based testing 方法。

这些内容用于回应 reviewer 的合理担忧，同时避免把未完成的实验写成已有结果。

## Compatible Check 的处理方式

Reviewer 对 scenario enhancement 中的随机组合提出了疑问：如果只是随机拼接 scenario，是否可能生成 meaningless scenario？

为回应这一点，v2 中将 `Compatible` check 写成一个轻量的 validity / compatibility 过滤机制。其核心作用是在接受一个候选 scenario 之前，检查它是否满足基本语义和工程约束。

可以理解为：

```text
不是所有随机组合都会被接受；
候选组合必须通过 compatibility check，才会进入增强后的 scenario set。
```

检查内容包括：

- shared input/output configuration 是否一致；
- temporal feasibility 是否满足 `T_sim`；
- 是否保留 requirement traceability；
- state-mode transition 是否合理；
- constraint 是否在 input bounds 下可满足。

这类补充不需要新增实验数据，但可以让方法描述更严谨。

## 阶段七：编译、验证并打包 v2

由于 v2 新增了不少 reviewer response 内容，初次编译后正文页数超出了 16 页限制。因此智能体进行了保守压缩：

- 压缩 algorithm 环境占用；
- 缩短 Related Work 部分重复表达；
- 调整 `main.tex` 中部分 spacing；
- 保留实质性 reviewer clarifications，不用删除关键修改来硬凑页数。

最终 v2 编译验证结果为：

```text
Pages: 19
Conclusion_page: 16
References_page: 17
ZIP_BUILD_OK
```

这表示：

- PDF 总页数 19；
- Conclusion 在第 16 页；
- References 从第 17 页开始；
- 正文仍满足 16 页限制；
- source zip 解压到干净目录后可以重新编译成功。

v2 的主要输出包括：

```text
v2/latex/
v2/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_v2.pdf
v2/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_latex_v2.zip
```

## 阶段八：生成审阅辅助文档

为了让修改过程可追溯，智能体额外生成了多份 Markdown 文档。

### 实际改动记录

```text
v2/actual_changes_v2.md
```

这份文档记录实际已经完成的修改，而不是只停留在计划层面。

### 段落级前后对比

```text
v2/paragraph_before_after_v2.md
```

这份文档按 LaTeX 文件分类，列出 v1 修改前段落和 v2 修改后段落，包含 27 个 change blocks，适合用于写 response letter 或人工复查。

### 未完全落实项报告

```text
v2/remaining_review_gaps_v2.md
```

这份文档区分了：

- 已经通过文字说明或方法澄清处理的问题；
- 只能作为 limitation / future work 的问题；
- 如果有时间可以继续增强的呈现问题。

其中未完全落实的主要是需要额外实验或统计的数据类要求，例如：

- 多 LLM 对比；
- 更多 SOTA Simulink / model-based testing 方法对比；
- STL constraint generation success rate；
- scenario enhancement validity rate。

### 工作流总结

```text
v2/agent_workflow_and_skills.md
```

这就是本文整理依据的原始总结文档，记录了本次智能体的工作阶段、对应 Skill、关键约束和输出文件索引。

## 关键工程约束

这次任务中有几个值得记录的工程细节。

### 中文路径问题

论文工作目录包含中文路径：

```text
/mnt/d/研究生/科研/论文/camera-ready
```

部分终端工具不允许直接将包含中文字符的路径作为 `workdir`，因此编译验证时采用了复制到 `/tmp/...` 的方式。例如：

```text
/tmp/camera_ready_v2_build
/tmp/camera_ready_v2_zip_verify
```

这样既避免路径问题，也能顺便完成 clean rebuild 验证。

### 不新增实验

用户明确要求不进行额外实验分析。因此，对于 reviewer 提出的多模型比较、success rate、validity rate 等要求，智能体没有虚构数据，而是通过以下方式处理：

- 方法说明；
- threat to validity；
- limitation；
- future work。

这保证了论文修改的学术诚信。

### 页数限制

Springer LNCS / TASE camera-ready 要求正文 16 页以内，不含 bibliography。v2 增补 reviewer response 后一度超页，因此需要重新压缩。

最终采用的策略是优先压缩排版和重复表达，而不是删掉关键 reviewer response。

## 最终产物索引

本次任务的主要产物如下。

### v1 产物

```text
v1/latex/
v1/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_v1.pdf
v1/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_latex_v1.zip
v1/changes_v1.md
```

### v2 产物

```text
v2/review_action_plan.md
v2/latex/
v2/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_v2.pdf
v2/LLM_Guided_Requirement_Scenario_Based_Testing_for_Simulink_Models_latex_v2.zip
v2/actual_changes_v2.md
v2/paragraph_before_after_v2.md
v2/remaining_review_gaps_v2.md
v2/agent_workflow_and_skills.md
```

## 总结

这次工作展示了一种比较实用的 AI 智能体协作方式：不是只让模型回答“应该怎么做”，而是让它在明确约束下直接操作文件、编译验证、生成产物，并把每一步记录下来。

在论文 camera-ready 场景中，这种方式尤其适合处理以下任务：

- LaTeX 源码清理；
- PDF 和 source zip 打包；
- 页数和引用检查；
- reviewer comments 修改规划；
- 多版本 LaTeX 维护；
- before/after 修改记录；
- remaining gaps 分析。

不过，智能体也不能替代作者的学术判断。比如 Springer 表格中的不确定字段、reviewer 要求的新实验、论文结论的边界，都需要作者最终确认。比较理想的模式是：让智能体负责繁琐、可验证、可重复的工程化工作，让作者负责内容取舍、学术判断和最终提交确认。