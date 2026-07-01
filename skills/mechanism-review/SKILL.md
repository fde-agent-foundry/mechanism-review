---
name: mechanism-review
description: Use when reviewing chemical or industrial process soft-sensor, what-if simulation, RTO, MPC/APC, or dynamic optimization results from loose Markdown reports or analysis notes that need mechanism explanation, sanity checks, causality assessment, evidence grading, or technical-report rewrite.
---

# Mechanism Review

## Overview

Use this skill after data analysis, soft sensing, simulation, RTO, MPC/APC, or dynamic optimization has produced a loose Markdown report or analysis summary. The skill must first digest the narrative, reconstruct the reviewable claims, then judge whether the conclusions can be defended by conservation laws, kinetics, thermodynamics, transport, equipment behavior, control dynamics, safety, economics, and data support.

Do not require a fixed input schema. The input may be a free-form Markdown report, exported project summary, meeting notes, mixed Chinese/English notes, or a draft application report. Treat the text as evidence to be interpreted, not as a template to be trusted.

Core rule:

```text
Do not polish weak evidence into strong causal claims.
```

## Language and Style Policy

Default to the user's language. If the user writes in Chinese or the input report is mainly Chinese, produce the entire review in Chinese, including section headings, table headers, verdicts, evidence grades, validation actions, and report-ready rewrite paragraphs.

Do not switch to English just because this skill's internal templates are written in English. Keep standard technical abbreviations such as `RTO`, `MPC`, `APC`, `Da`, `K_eq`, `Ea`, `Y_nBA`, and tag names unchanged when they are clearer that way.

For Chinese outputs:

- Use concise technical-report Chinese, not conversational translation.
- Prefer terms such as `事实`, `解释`, `结论`, `机理一致性`, `证据等级`, `操作风险`, `验证动作`.
- Use verdict labels as `自洽`, `有条件自洽`, `不自洽`, or `证据不足`.
- Use confidence labels as `高`, `中`, `中低`, or `低`.
- When rewriting report text, write paragraphs that can be directly inserted into a Chinese technical report.
- If creating or editing an output artifact for a Chinese user, make the artifact body Chinese by default. Do not create English report drafts for Chinese source material unless the user explicitly asks for English.

Chinese names for the M-CHECK blocks:

| English block | Chinese output heading |
|---------------|------------------------|
| KPI Decomposition | KPI 拆解 |
| Material Balance | 物料衡算 |
| Chemistry and Kinetics | 反应化学与动力学 |
| Thermodynamics and Equilibrium | 热力学与平衡 |
| Residence Time, Transport, and Dimensionless Groups | 停留时间、传递与无量纲数 |
| Phase Behavior and Separation Coupling | 相态行为与分离耦合 |
| Heat Balance and Thermal Risk | 热量衡算与热风险 |
| Catalyst, Impurities, Fouling, and Deactivation | 催化剂、杂质、结垢与失活 |
| Dynamics and Controllability | 动态响应与可控性 |
| Operating Window, Safety, Quality, and Compliance | 操作窗口、安全、质量与合规 |
| Economics | 经济性 |
| Data Support and Extrapolation Risk | 数据支撑与外推风险 |

## Scope

Do:

- Read loose Markdown and reconstruct process context.
- Separate facts, interpretations, and conclusions.
- Extract reviewable claims or optimization recommendations.
- Ask the user to confirm the most important review focus after digestion, while allowing low-confidence review to continue if no additional evidence is available.
- Apply mechanism-consistency checks.
- Grade evidence and identify missing information.
- Produce technical-report-style rewrite paragraphs for claims that are self-consistent or conditionally self-consistent.

Do not:

- Retrain models or rerun the original soft-sensor/simulation/optimization workflow unless explicitly asked.
- Treat model correlation as mechanism causality.
- Accept the optimizer's target point without checking constraints and evidence.
- Force every process into a reaction-only explanation.
- Make unsupported mechanism claims sound certain.

## Stage 0: Context Digestion

Read the full input first. Before applying mechanism checks, reconstruct the project logic.

Extract:

- Process context: unit, product, process type, equipment boundary, feeds, products, recycle, purge, and key disturbances.
- Business/modeling objective: yield, conversion, selectivity, purity, throughput, energy, emissions, stability, cost, or multi-objective goal.
- KPI definition: formula, units, calculation basis, data sources, and distinction from common plant indicators.
- Analysis chain: soft sensor, what-if simulation, optimization, RTO/MPC/APC prescription, dynamic horizon, and target variables.
- Current point and target point: manipulated variables, controlled variables, constraints, predicted KPI delta, and operating segment.
- Evidence: model metrics, lab data, DCS tags, composition/recycle data, figures, bidirectional perturbations, neighborhood samples, historical windows.
- Mechanism assumptions: stoichiometry, main/side reactions, phase behavior, catalyst, heat effects, separations, transport limits, safety boundaries.
- Missing information: split into critical gaps and non-blocking gaps.

Separate each important statement into one of three types:

```text
Fact: measured, calculated, or reported value.
Interpretation: proposed mechanism or causal explanation.
Conclusion: recommendation, verdict, or business claim.
```

## Stage 0 Output: Extracted Review Targets

Always produce a compact extraction table before the review.

Use this shape in English outputs:

```text
| ID | Claim or recommendation | Variables | KPI impact | Evidence mentioned | Initial confidence |
|----|-------------------------|-----------|------------|--------------------|-------------------|
| C1 | ...                     | ...       | ...        | ...                | low/medium/high   |
```

Use this shape in Chinese outputs:

```text
| ID | 论断或建议 | 相关变量 | KPI 影响 | 已有证据 | 初始置信度 |
|----|------------|----------|----------|----------|------------|
| C1 | ...        | ...      | ...      | ...      | 高/中/中低/低 |
```

If the input is long, group related claims rather than listing every sentence. Prefer claims that affect the final recommendation, mechanism story, or operating decision.

## Stage 0.5: User Alignment and Evidence Request

After Stage 0, ask the user to confirm the review focus and request the smallest evidence additions that would materially improve the mechanism review. This stage is non-blocking: if the user has no additional evidence or asks to continue, proceed to Stage 1 and lower the evidence grade for claims that depend on missing information.

Use Stage 0.5 especially when the input relies on screenshots, OCR, plots, or narrative claims without numeric summaries.

Output in English when the user/input is English:

```text
## Stage 0.5. User Alignment and Evidence Request

### Extracted Focus Candidates
List 5-8 possible focal directions from the input.

### Recommended Focus
Recommend the 2-3 directions most worth emphasizing in the final review, with short reasons.

### Evidence Needed Before Stage 1
Request at most 3 evidence additions that would most improve confidence.

### If Evidence Is Not Available
Explain how the review will continue and which conclusions will be downgraded.
```

Output in Chinese when the user/input is Chinese:

```text
## Stage 0.5. 用户对齐与证据补充

### 可重点审查的方向
列出从输入中抽取出的 5-8 个候选方向。

### 建议优先关注
推荐最值得放进最终审查的 2-3 个方向，并给出简短理由。

### 进入 Stage 1 前最需要的证据
最多请求 3 项最能提高置信度的补充证据。

### 如果暂时没有补充证据
说明会如何继续审查，以及哪些结论需要降级。
```

Useful evidence requests:

- Numeric what-if table for screenshots or OCR-only simulation evidence.
- Current point versus target point table for optimization cases.
- KPI formula and tag/data-source mapping if not explicit.
- Target-point neighborhood sample count and historical operating range.
- Lab confirmation, soft-sensor residuals, or time-alignment notes.
- Known mechanism facts such as stoichiometry, main/side reactions, catalyst role, phase behavior, heat effects, or equipment limits.

For what-if evidence, request this compact shape in English outputs:

```text
| Case | Baseline period | Variable | Step | Direction | KPI baseline | KPI final/peak | Response time | Notes |
|------|-----------------|----------|------|-----------|--------------|----------------|---------------|-------|
```

Use this shape in Chinese outputs:

```text
| Case | 基线区间 | 变量 | 扰动步长 | 方向 | KPI 基线值 | KPI 终值/峰值 | 响应时间 | 备注 |
|------|----------|------|----------|------|------------|---------------|----------|------|
```

For optimization evidence, request this compact shape in English outputs:

```text
| Case | Current point | Target point | Predicted KPI gain | Active constraints | Neighborhood samples | Notes |
|------|---------------|--------------|--------------------|-------------------|----------------------|-------|
```

Use this shape in Chinese outputs:

```text
| Case | 当前点 | 目标点 | 预测 KPI 增益 | 活跃约束 | 目标点邻域样本数 | 备注 |
|------|--------|--------|---------------|----------|------------------|------|
```

Do not ask for broad rewrites of the input report. Ask only for the smallest missing evidence that affects focus, causality, or confidence.

## Stage 1: Mechanism Review

Apply the M-CHECK sequence to each major claim or recommendation. Skip a block only when it is clearly irrelevant, and say why.

If Stage 0.5 evidence is not provided, continue the review using the available input. Explicitly mark screenshot/OCR-only or narrative-only claims as lower confidence unless they are supported by independent numeric evidence elsewhere.

### 1. KPI Decomposition

Decompose the optimized KPI into physical components before explaining direction.

Examples:

```math
Yield \approx Conversion \times Selectivity \times Recovery
```

```math
Energy\ intensity = \frac{Heat\ duty}{Product\ amount}
```

```math
Purity \approx Separation\ driving\ force \times Stage\ efficiency \times Recycle\ effect
```

Ask:

- Which component actually improves?
- Could the KPI change be a concentration, recycle, dilution, inventory, or accounting effect?
- Does the model target match the business target?

### 2. Material Balance

Check whether the explanation obeys component and elemental conservation.

Look for:

- Feed, product, recycle, purge, vent, wastewater, waste solid, and heavy-end routes.
- Single-pass conversion versus overall conversion.
- Product recovery versus product concentration.
- Carbon, nitrogen, sulfur, chlorine, solvent, water, or catalyst balances as applicable.
- Losses hidden in recycle loops or sparse lab measurements.

Weaken explanations that improve a KPI without a credible material route.

### 3. Chemistry and Kinetics

For reacting systems, identify main reactions, side reactions, rate-limiting steps, catalyst dependence, and temperature sensitivity.

Use:

```math
r_i = k_i \prod_j C_j^{\alpha_{ij}}
```

```math
k_i = A_i \exp \left(-\frac{E_{a,i}}{RT}\right)
```

Review:

- Main reaction rate change.
- Side/main reaction rate ratio.
- Activation energy differences.
- Reaction orders and concentration effects.
- Effective catalyst concentration, deactivation, poisoning, dilution, acidity/basicity, active-site limits.
- Whether the result is kinetic-limited, equilibrium-limited, transport-limited, or constraint-limited.

Do not assume higher temperature or more catalyst is beneficial. Decide from conversion, selectivity, equilibrium, transport, and safety.

### 4. Thermodynamics and Equilibrium

Check whether the target changes equilibrium driving force or only the rate of approach.

Use when relevant:

```math
\frac{Q}{K_{eq}}
```

```math
\Delta G = \Delta G^\circ + RT \ln Q
```

Review:

- Equilibrium constant, reaction quotient, and distance from equilibrium.
- Heat of reaction and temperature effect on equilibrium.
- Le Chatelier effects from feed ratio, pressure, product removal, solvent, or water removal.
- Whether catalyst changes equilibrium position; normally it does not.
- Competing thermodynamic and kinetic incentives.

### 5. Residence Time, Transport, and Dimensionless Groups

Connect chemistry to equipment and flow.

Use:

```math
Da = k \tau
```

where `k` is the relevant first-order or pseudo-first-order effective rate constant and `tau` is residence time.

Review:

- Residence time, space velocity, holdup, liquid level, gas/liquid flow, recycle ratio.
- Mixing time versus reaction time.
- Heat transfer and mass transfer limits.
- Diffusion, film resistance, interphase transfer, fouling, maldistribution.
- Dimensionless numbers when useful: Damkohler, Reynolds, Peclet, Sherwood, Nusselt, Weber, Froude.

Interpretation examples:

- High Da: reaction capacity may be redundant; selectivity, equilibrium, or separation can dominate.
- Da near 1: small changes in temperature, catalyst, or load can be binding.
- Low Da: optimization that reduces rate or residence time is risky unless another mechanism compensates.

### 6. Phase Behavior and Separation Coupling

Review phase equilibrium and separation effects, especially in coupled reaction-separation systems.

Look for:

- Vapor-liquid, liquid-liquid, solid-liquid, adsorption, membrane, azeotropic, or crystallization behavior.
- Relative volatility, K-values, reflux, boilup, pressure, product removal, solvent loading, extraction distribution, supersaturation.
- Product concentration versus true yield or recovery.
- Recycle composition effects and hidden inventory shifts.
- Downstream separation load caused by upstream optimization.

### 7. Heat Balance and Thermal Risk

Check whether the target is thermally feasible and safe.

Review:

- Reaction heat, adiabatic temperature rise, hot spots, cooling capacity, reboiler duty, condenser duty.
- Temperature limits for product quality, side reactions, polymerization, decomposition, corrosion, catalyst stability.
- Local temperature risk, not only average temperature.
- Heat removal margin under disturbances or load changes.

### 8. Catalyst, Impurities, Fouling, and Deactivation

Review mechanisms that may not show up in short-term model predictions.

Look for:

- Catalyst aging, poisoning, leaching, neutralization, water sensitivity, acidity/basicity drift, active-site saturation.
- Polymerization, coking, fouling, scaling, salt formation, corrosion, solids, high-boiling byproducts.
- Impurity accumulation in recycle loops.
- Long-run stability versus short-run KPI improvement.

### 9. Dynamics and Controllability

RTO or dynamic optimization gives targets, but the plant must travel there.

Review:

- Response direction, gain sign, dead time, time constant, overshoot, inverse response, oscillation risk.
- MV/CV pairing, valve limits, controller saturation, APC/MPC constraint activity.
- Whether the target path is staged enough for lab confirmation and safety.
- Whether the predicted improvement appears before or after known process delays.

### 10. Operating Window, Safety, Quality, and Compliance

Check hard and soft boundaries:

- Temperature, pressure, level, flow, flooding, weeping, pressure drop, pump NPSH, compressor surge, relief limits.
- Product specs, impurity specs, color, acidity, water, polymer, heavy ends.
- Environmental limits: COD, VOC, SOx/NOx, wastewater, vent, waste solids.
- Site procedures, alarm limits, material compatibility, corrosion windows.

### 11. Economics

Explain whether the improvement is valuable after costs.

Include as relevant:

- Feedstock, catalyst, solvent, neutralizer, steam, power, cooling water, hydrogen, nitrogen, oxygen, waste treatment.
- Product value, off-spec risk, recycle energy, bottleneck relief, capacity increase.
- Marginal profit, not only model KPI.

Flag recommendations that improve yield or purity by spending more than they earn.

### 12. Data Support and Extrapolation Risk

Grade evidence separately from mechanism plausibility.

Review:

- Training coverage and target-point neighborhood sample count.
- Whether target variables are inside historical operating bounds.
- Lab frequency and soft-sensor residuals.
- Feature leakage, sparse composition data, recycle inventory lag, unmeasured disturbances.
- Bidirectional what-if fingerprints and historical step responses.
- Whether the model is interpolating, mildly extrapolating, or extrapolating beyond credible data.

## Stage 1 Output: Review Report

Use this structure in English outputs:

```text
## Stage 1. Mechanism Review

### Verdict
Self-consistent / conditionally self-consistent / not self-consistent / insufficient evidence.

### Strong Points
Mechanism claims that are supported by multiple independent checks.

### Weak Points and Contradictions
Claims that conflict with mechanism, data, operating limits, or each other.

### Missing Evidence
Critical gaps and non-blocking gaps.

### Operational Risks
Dynamics, controllability, equipment window, safety, quality, catalyst/fouling, long-run stability.

### Evidence Grade
Data coverage, extrapolation risk, soft-sensor uncertainty, lab support, and confidence level.

### Validation Actions
The smallest plant, lab, or analysis checks that would most improve confidence.
```

Use this structure in Chinese outputs:

```text
## Stage 1. 机理审查

### 总体判断
自洽 / 有条件自洽 / 不自洽 / 证据不足。

### 强支撑点
有哪些机理判断得到了多条独立证据支持。

### 弱点与矛盾
哪些结论与机理、数据、操作边界或其它结论存在冲突。

### 缺失证据
区分关键缺口和非关键缺口。

### 操作风险
动态、可控性、设备窗口、安全、质量、催化剂/结垢、长期稳定性风险。

### 证据等级
数据覆盖、外推风险、软测量不确定性、实验室支撑和总体置信度。

### 验证动作
最小化的现场、实验室或分析验证动作。
```

## Verdict Scale

- `Self-consistent`: mechanism direction, constraints, data support, and economics broadly agree.
- `Conditionally self-consistent`: plausible if stated assumptions hold, but needs validation or has sparse evidence.
- `Not self-consistent`: conflicts with conservation, known chemistry, operating limits, or observed plant response.
- `Insufficient evidence`: cannot be assessed without missing chemistry, composition, lab, or operating-window data.

## Stage 2: Report-Ready Technical Rewrite

After Stage 1, write technical-report-style language for the final report. Use rigorous, restrained wording suitable for process engineers and technical reviewers.

Rules:

- Rewrite only claims that are self-consistent or conditionally self-consistent.
- Downgrade certainty when evidence is limited.
- Preserve assumptions and boundary conditions.
- Prefer "supports", "is consistent with", "suggests", "under the current operating window", and "requires validation" over "proves" unless proof is genuinely available.
- Do not hide contradictions or missing evidence.
- Do not turn a model correlation into a deterministic mechanism.

Use this structure in English outputs:

```text
## Stage 2. Report-Ready Technical Rewrite

### Ready to Use
Paragraphs that can be inserted into a technical report.

### Downgraded Wording Recommended
Original or implied strong claim -> safer technical wording.

### Not Recommended for Report
Claims that should not be included unless additional evidence is obtained.
```

Use this structure in Chinese outputs:

```text
## Stage 2. 报告可用技术表述

### 可直接使用
可以插入技术报告的段落。

### 建议降级表述
原始或隐含的强说法 -> 更稳妥的技术表述。

### 暂不建议写入报告
除非补充证据，否则不建议纳入报告的论断。
```

## Common Mistakes

- Producing English deliverables for Chinese inputs or Chinese users when English was not requested.
- Explaining concentration change as yield change without closing recycle and recovery balances.
- Treating catalyst as changing equilibrium rather than rate.
- Treating higher conversion as always better when selectivity falls.
- Ignoring water, solvent, purge, or recycle inventory effects.
- Using average temperature while the mechanism is controlled by hot spots.
- Treating an optimized target point as executable without checking dynamic path and control constraints.
- Calling a model result mechanistic because it matches one expected direction; require multiple independent checks.
- Rewriting weak evidence as confident causal language.
