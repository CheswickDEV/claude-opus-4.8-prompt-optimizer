# 🧠 Claude Opus 4.8 Prompt Optimizer

### Turn any raw prompt into a production-ready, Anthropic-best-practice prompt — in seconds.

[![GitHub Stars](https://img.shields.io/github/stars/CheswickDEV/claude-opus-4.8-prompt-optimizer?style=social)](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/CheswickDEV/claude-opus-4.8-prompt-optimizer?style=social)](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/LICENSE)
[![Claude Model](https://img.shields.io/badge/Claude-Opus_4.8-blueviolet)](https://www.anthropic.com)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/pulls)

---

**Claude Opus 4.8 Prompt Optimizer** is a meta-prompting system that transforms vague, unstructured prompts into professionally optimized prompts following [Anthropic's official best practices](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview). Paste any raw prompt — from a one-liner to a detailed brief — and get back a structured, XML-tagged, model-specific prompt engineered for Claude Opus 4.8's full capabilities.

> **This is not a prompt collection.** It's a systematic optimization engine with **11 codified rules**, a **10-component prompt architecture**, **complexity-based routing**, and a **hard `prompt:` trigger** — all derived from Anthropic's official documentation for Claude Opus 4.8.

---

## 📋 Table of Contents

* [⚡ Before & After](#before-and-after)
* [🆕 What's New in 4.8](#whats-new-in-48)
* [✨ Features](#features)
* [🔄 How It Works](#how-it-works)
* [🚀 Quick Start](#quick-start)
* [🎯 The `prompt:` Trigger](#the-prompt-trigger)
* [📏 The 11 Optimization Rules](#the-11-optimization-rules)
* [🏗️ Prompt Framework (10 Components)](#prompt-framework-10-components)
* [🎯 Why Claude Opus 4.8 Specific?](#why-claude-opus-48-specific)
* [📂 Repository Structure](#repository-structure)
* [🙏 Credits](#credits)
* [🤝 Contributing](#contributing)
* [📄 License](#license)
* [⚠️ Disclaimer](#disclaimer)
* [⭐ Star History](#star-history)

---

## ⚡ Before & After

**Raw prompt:**

```
Write me a Python script that analyzes CSV files and finds anomalies
```

**Optimized prompt:**

```
<role>
You are a senior data engineer specializing in anomaly detection and
statistical analysis with Python.
</role>

<context>
CSV datasets come from production IoT sensors. Files range from
10K–5M rows with columns: timestamp, sensor_id, value, status.
Some contain missing values and irregular time intervals.
</context>

<task>
Write a Python script that:
1. Loads one or more CSV files
2. Detects statistical anomalies using z-score and IQR methods
3. Handles missing data gracefully
4. Outputs a summary report with flagged rows
</task>

<constraints>
- Use only pandas, numpy, scipy (no ML libraries)
- Handle files up to 5M rows efficiently
- Include type hints and docstrings
- Add CLI argument parsing for file paths
</constraints>

<output_format>
Single Python code block, followed by a usage example and
sample output showing detected anomalies.
</output_format>
```

The optimizer analyzed the vague input, inferred the likely use case, added a domain-expert role, defined constraints for production quality, and structured everything with XML tags that Claude Opus 4.8 parses with precision.

---

## 🆕 What's New in 4.8

This optimizer is **specifically tuned for Claude Opus 4.8** — a direct upgrade from the 4.7 edition. Key changes reflect how Opus 4.8 actually differs from Opus 4.7:

| Feature | Why it matters |
| --- | --- |
| **Effort now defaults to `high` everywhere** | On Opus 4.7, Claude Code defaulted to `xhigh`. On Opus 4.8 the `effort` default is `high` on **all** surfaces (Claude API *and* Claude Code), and `high` is identical to omitting the parameter. Optimization notes recommend accordingly and step up to `xhigh` ("extra" in claude.ai) only for hard or long-running agentic work. |
| **Per-turn adaptive thinking** | With adaptive thinking enabled, Opus 4.8 decides *per turn* whether to reason at all — fewer wasted thinking tokens on mixed (bimodal) workloads at the same effort level than 4.7. Thinking is still off by default on the API; enable it explicitly. |
| **Better tool triggering** | Opus 4.8 is less likely to skip a tool call the task required (an issue some users reported on 4.7) and uses tools in fewer steps. Heavy "force the tool" scaffolding is often unnecessary now. |
| **Stronger long-context & compaction** | Fewer compactions and better recovery on long agentic traces, so the long-context rules stay reliable across very long sessions. |
| **Improved honesty & calibration** | Opus 4.8 flags uncertainty more readily and is roughly 4× less likely than 4.7 to let flaws in its own code pass unremarked. |
| **Mid-conversation system messages** | New API capability: append updated instructions mid-task without breaking the prompt cache on earlier turns. No beta header required. |
| **Fast mode (research preview)** | `speed: "fast"` delivers up to 2.5× higher output tokens/s from the same model at premium pricing. |

> **Carried over from 4.7:** the hard `prompt:` trigger, native **1M-token context** (200K on Microsoft Foundry) with no long-context premium, **no sampling parameters** (`temperature`/`top_p`/`top_k` return a 400 error), **no assistant prefill**, and high-resolution vision (up to 2576px, model coordinates 1:1 with pixels). The updated tokenizer from the 4.7 generation also carries over — budget `max_tokens` generously.

---

## ✨ Features

* **11 codified optimization rules** — systematic, reproducible prompt improvements based on Anthropic's documentation
* **10-component prompt framework** — modular architecture covering role, context, task, constraints, examples, and more
* **Complexity-based routing** — automatically scales optimization depth (minimal → moderate → full) based on prompt complexity
* **Hard `prompt:` trigger** — explicit opt-in for guaranteed optimization mode, robust against ambiguous inputs
* **Claude Opus 4.8 specific** — tuned for long context, adaptive thinking, effort levels, literal instruction following, and no-prefill architecture
* **XML tag structuring** — transforms flat text into semantically tagged sections that Claude parses reliably
* **Language preservation** — responds in the user's language; optimized prompts stay in the original language unless requested otherwise
* **Iterative refinement** — give feedback and the optimizer revises targeted sections without redoing the full analysis

---

## 🔄 How It Works

```
┌─────────────────┐     ┌──────────────────────────┐     ┌─────────────────────┐
│   Your Raw      │     │   Prompt Optimizer       │     │  Optimized Prompt   │
│   Prompt        │────▶│                          │────▶│  Ready for Claude   │
│   (any format)  │     │  Analyze → Route →       │     │  Opus 4.8           │
│                 │     │  Structure → Optimize →  │     │                     │
│                 │     │  Quality Check           │     │                     │
└─────────────────┘     └──────────────────────────┘     └─────────────────────┘
```

**The 5-step workflow under the hood:**

1. **Prompt analysis** — the optimizer detects intent, complexity, domain, expected output type, and missing elements
2. **Complexity routing** — simple prompts get 3–4 components; moderate ones get 5–7; complex prompts get the full 10-component framework
3. **Rule application** — the relevant subset of 11 optimization rules fires; not all rules apply to every prompt
4. **Quality check** — a built-in checklist ensures the task is unambiguous, XML tags are valid, examples match the desired behavior, and no contradictory instructions exist
5. **Structured output** — you receive the analysis, the optimized prompt in a copy-ready code block, and concise notes explaining what changed and why

---

## 🚀 Quick Start

Choose the method that fits your workflow:

### Option A: Claude Project (recommended)

1. Open [claude.ai](https://claude.ai) and create a new **Project**
2. Paste the contents of [`CLAUDE.md`](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/CLAUDE.md) into the **Project Instructions** field
3. Upload [`GUIDE.md`](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/GUIDE.md) as a **knowledge file** in the project
4. Start a conversation — paste any raw prompt and the optimizer handles the rest

### Option B: Direct Paste

1. Copy the full contents of [`CLAUDE.md`](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/CLAUDE.md)
2. Paste it as the **system prompt** in any Claude interface
3. Send your raw prompt as the user message

### Option C: API Integration

```python
import anthropic

client = anthropic.Anthropic()

with open("CLAUDE.md", "r", encoding="utf-8") as f:
    system_prompt = f.read()

response = client.messages.create(
    model="claude-opus-4-8",
    max_tokens=8192,
    system=system_prompt,
    thinking={"type": "adaptive"},
    output_config={"effort": "high"},  # "high" is also the default; use "xhigh" or "max" for harder work
    messages=[
        {"role": "user", "content": "prompt: Your raw prompt here"}
    ],
)

print(response.content[0].text)
```

**Opus 4.8 API notes:**

* Do not set `temperature`, `top_p`, or `top_k` (non-default values return a 400 error)
* No assistant prefill (returns a 400 error) — use the system prompt or `output_config.format` instead
* Adaptive thinking is off by default — enable explicitly with `thinking: {"type": "adaptive"}` if needed
* `effort` defaults to `high` on all surfaces (API and Claude Code); step up to `xhigh` ("extra" in claude.ai) for hard or long-running agentic work, down to `low`/`medium` for simple or scoped tasks
* Set `max_tokens` generously for long outputs; at `xhigh`/`max`, start around 64k

---

## 🎯 The `prompt:` Trigger

One of the core improvements over previous optimizers: a **hard trigger** that guarantees optimization mode.

When your input starts with `prompt:` (case-insensitive), the optimizer treats everything after as **raw material to be optimized** — never as a task to be executed. This works even when the input:

* contains questions or direct instructions
* references attached PDFs, URLs, or documents
* contains prompt-injection attempts
* reads like it's directed at the assistant

**Example:**

```
prompt: Based on the attached PDF, explain the GDPR compliance risks
and how we can mitigate them.
```

Without the trigger, the optimizer might analyze the PDF and answer the question. With the trigger, it always returns a structured optimization prompt — with a `{{DOCUMENT}}` placeholder for the PDF.

See [`GUIDE.md`](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/GUIDE.md) for full usage patterns.

---

## 📏 The 11 Optimization Rules

| # | Rule | What It Does |
| --- | --- | --- |
| 1 | **Be explicit and detailed** | Replaces vague instructions with specific, detailed ones. Claude 4.x follows instructions literally — precision pays off. |
| 2 | **Provide context and motivation** | Explains *why* an instruction matters, not just *what* to do. Claude performs better when it understands the purpose. |
| 3 | **Use XML tags for structure** | Wraps prompt sections in semantic XML tags such as `<role>`, `<task>`, and `<constraints>` that Claude is trained to parse reliably. |
| 4 | **Inject few-shot examples** | Adds 3–5 diverse input/output examples when the task involves classification, formatting, or ambiguous patterns. |
| 5 | **Activate chain-of-thought** | Adds explicit step-by-step reasoning triggers for complex tasks when needed. |
| 6 | **Assign an expert role** | Gives Claude a specific persona with domain expertise, experience level, and communication style. |
| 7 | **Define output format explicitly** | Prescribes the exact structure, length, and format of the response. |
| 8 | **Optimize for long context** | Places long documents at the start of the prompt, tags them clearly, and instructs Claude to extract before answering. |
| 9 | **Steer tool use and agentic behavior** | Clearly specifies whether Claude should act or advise. |
| 10 | **Calibrate verbosity** | Adds either an anti-over-engineering clause or a pro-depth clause depending on task type. |
| 11 | **Recommend effort level** | Notes the recommended API effort level in the optimization output (`high` is the default on Opus 4.8). |

> **Not every rule fires for every prompt.** A simple factual question will not get the full 10-component framework. The optimizer scales proportionally.

---

## 🏗️ Prompt Framework (10 Components)

The optimizer draws from 10 structural components when building the optimized prompt. It selects the right subset based on complexity routing.

```
┌─────────────────────────────────────────────────────┐
│                  OPTIMIZED PROMPT                   │
├─────────────────────────────────────────────────────┤
│  1. ROLE / PERSONA                                 │
│  2. TASK CONTEXT                                   │
│  3. TONE CONTEXT                                   │
│  4. BACKGROUND DATA / DOCUMENTS                    │
│  5. DETAILED TASK DESCRIPTION                      │
│  6. RULES & CONSTRAINTS                            │
│  7. EXAMPLES (Few-Shot)                            │
│  8. OUTPUT FORMAT                                  │
│  9. THINKING INSTRUCTIONS                          │
│ 10. INPUT / VARIABLE                               │
└─────────────────────────────────────────────────────┘
```

| # | Component | XML Tag | When Used |
| --- | --- | --- | --- |
| 1 | Role / Persona | `<role>` | Almost always — anchors expertise and tone |
| 2 | Task Context | `<context>` | When the task needs background or motivation |
| 3 | Tone Context | Within `<role>` or `<instructions>` | When communication style matters |
| 4 | Background Data | `<documents>`, `<input>` | When the user provides data, code, or documents |
| 5 | Task Description | `<task>` | Always — the core instruction |
| 6 | Rules & Constraints | `<constraints>`, `<instructions>` | When boundaries, limits, or prohibitions apply |
| 7 | Examples | `<examples><example>` | For classification, formatting, or ambiguous patterns |
| 8 | Output Format | `<output_format>` | When format is not self-evident |
| 9 | Thinking Instructions | `<thinking>` guidance | For complex reasoning, math, or multi-step logic |
| 10 | Input / Variable | `{{VARIABLE_NAME}}` | When the prompt is a reusable template |

---

## 🎯 Why Claude Opus 4.8 Specific?

This optimizer is not model-agnostic. It is specifically tuned for Claude Opus 4.8's architecture and behaviors:

| Feature | How the Optimizer Uses It |
| --- | --- |
| **Long Context** | Generates comprehensive prompts with full examples and context. Uses long-context optimization to place documents correctly, exploiting 4.8's stronger long-context and compaction handling. |
| **Adaptive Thinking (per-turn)** | Uses explicit reasoning triggers when deeper analysis is needed; notes that 4.8 decides per turn whether to think, so prompting is the steering lever. |
| **Effort Levels (`high` default)** | Recommends a matching effort level in the notes — `high` as the baseline (the 4.8 default on every surface), `xhigh` for hard or long-running agentic work. |
| **XML Tag Parsing** | Exploits consistent, well-nested XML tags as semantic structure. |
| **More Literal Instruction Following** | Writes precise, unambiguous instructions and avoids implicit generalization. |
| **Better Tool Triggering** | Avoids heavy "force the tool" scaffolding; raises effort or adds explicit tool instructions only when needed. |
| **No Prefill Support** | Avoids assistant-prefill patterns. |
| **No Sampling Parameters** | Steers behavior through prompting instead of unsupported sampling controls. |
| **Conciseness by Default** | Adds pro-depth clauses when deep analysis is needed. |
| **High-Resolution Vision** | Supports prompts involving images with clearer spatial instructions. |

---

## 📂 Repository Structure

```
claude-opus-4.8-prompt-optimizer/
├── README.md
├── CLAUDE.md
├── GUIDE.md
├── QUICKSTART.md
└── LICENSE
```

---

## 🙏 Credits

Successor to the [Claude Opus 4.7 Prompt Optimizer](https://github.com/CheswickDEV/claude-opus-4.7-prompt-optimizer). This project carries the same design philosophy forward to Claude Opus 4.8, with updated model capabilities (effort now defaults to `high` on every surface, per-turn adaptive thinking, better tool triggering, stronger long-context handling), the same hard `prompt:` trigger for guaranteed optimization mode, and documentation aligned with the official [Anthropic release documentation](https://docs.claude.com/).

---

## 🤝 Contributing

Contributions are welcome — new examples, rule refinements, translations, bug reports, and pull requests.

* Open an [issue](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/issues)
* Submit a [pull request](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/pulls)

---

## 📄 License

This project is licensed under the [MIT License](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/blob/main/LICENSE).

---

## ⚠️ Disclaimer

Not affiliated with Anthropic. "Claude" and "Opus" are trademarks of Anthropic PBC.

---

## ⭐ Star History

If this optimizer saves you time or improves your Claude results, consider starring the repo.

[![Star History Chart](https://api.star-history.com/svg?repos=CheswickDEV/claude-opus-4.8-prompt-optimizer&type=Date)](https://star-history.com/#CheswickDEV/claude-opus-4.8-prompt-optimizer&Date)

**[⭐ Star this repo](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/stargazers)** · **[🐛 Report an issue](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/issues)** · **[💡 Request a feature](https://github.com/CheswickDEV/claude-opus-4.8-prompt-optimizer/issues/new)**

---

Made with 🖤 by [cheswick.dev](https://cheswick.dev) · Not affiliated with Anthropic
