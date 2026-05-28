# Quickstart Example

This example shows how the Claude Opus 4.8 Prompt Optimizer turns a minimal, informal input into a production-ready, XML-structured prompt.

---

## Unoptimized Input

```
prompt: Give me an analysis of our Q4 numbers and tell me what we can do better
```

---

## Optimizer Output

### 📊 Prompt Analysis

- **Intent**: Structured analysis of Q4 business figures with actionable recommendations
- **Complexity**: Moderate
- **Domain**: Business / Finance / Analysis
- **Applied Rules**: 1 (explicitness), 3 (XML structure), 6 (role), 7 (format), 10 (verbosity calibration: pro-depth)

---

### 🎯 Optimized Prompt

```
<role>
You are an experienced business analyst with a background in controlling and
strategy consulting. You analyze business figures on a factual basis, recognize
patterns, and derive prioritized, actionable recommendations from them.
</role>

<context>
The user provides Q4 figures from a company and wants a structured analysis
with concrete improvement suggestions. The audience for the analysis is
executive leadership.
</context>

<task>
Analyze the provided Q4 figures and create a management-ready executive summary
with prioritized recommendations.
</task>

<input>
{{Q4_FIGURES}}
</input>

<instructions>
- Identify three to five key findings from the figures
- For each finding, explain: what the number shows, what causes are likely, what risk or opportunity it implies
- Derive one concrete recommendation per finding (who, what, by when)
- Prioritize all recommendations in a table by impact (high/medium/low) and effort (high/medium/low)
- Analyze thoroughly — one-line recommendations will not suffice
</instructions>

<output_format>
Structured report with the following sections:

1. Executive Summary (maximum five sentences, core message)
2. Key Findings (per finding: observation, interpretation, implication)
3. Prioritization table (columns: recommendation, impact, effort, priority, suggested owner)
4. Next Steps (three to five concrete actions for the next 30 days)

Prose in sections 1, 2, and 4. Markdown table in section 3. No graphics.
</output_format>

<constraints>
- No speculative numbers — work exclusively with the provided data
- For missing information: flag explicitly as an information gap
- No buzzwords, no marketing vocabulary
- Use hedged language when uncertain ("suggests", "indicates")
</constraints>
```

---

### 💡 Optimization Notes

- Role "business analyst" with controlling focus added for fact-based orientation
- Audience (executive leadership) explicitly named → affects tone and abstraction level
- Report structure predefined, including a prioritization table
- Anti-hallucination constraint added against speculative numbers
- `{{Q4_FIGURES}}` as a placeholder for the actual dataset
- Pro-depth clause instead of anti-over-engineering — the task is analytically open-ended
- **Recommended effort level**: `high` (knowledge work with multi-step reasoning). On Opus 4.8, `high` is the default on every surface (Claude API and Claude Code) and is identical to omitting the parameter — so this prompt needs no effort override. Step up to `xhigh` ("extra" in claude.ai) only for harder or long-running agentic work.
- For API use: enable `thinking: {"type": "adaptive"}` if you want the model to reason before answering (it is off by default on the API). With adaptive thinking on, Opus 4.8 decides per turn whether to reason, so it will think on this open-ended analysis and skip thinking on trivial turns. If you want a visible reasoning stream, also set `display: "summarized"`.
