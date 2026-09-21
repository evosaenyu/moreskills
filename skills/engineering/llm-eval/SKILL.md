---
name: llm-eval
description: "Eval a harness against its base model. Use when the user wants evals of an agent, tool loop, or LLM wrapper; asks whether the harness beats the model; or wants latency, token usage, accuracy, or network requests measured on an LLM run. Do not use for ordinary unit tests (call tdd) or for evaluating JavaScript."
---

# LLM Eval

An **eval** is a paired run of the same task: once through the **harness** (tools, skills, loop), once through the **baseline** (the same model, no tools). The score that matters is the delta.

Never score a harness in isolation. A number with no baseline is not an eval.

When exploring the codebase, read `CONTEXT.md` (if it exists) so task names match the project's domain language.

For ordinary code (a function, an HTTP handler, a module with a seam), call the Skill tool with "tdd".

## The four axes

Every eval records all four, on both sides.

| Axis | What you count |
| --- | --- |
| **Latency** | Wall-clock from prompt in to final answer |
| **Tokens** | Input tokens plus output tokens, summed across every model provider request. Harness totals include tool results fed back. |
| **Accuracy** | Whether the answer is right, against an independent source of truth |
| **Network requests** | Every outbound round trip: each model provider request, plus each HTTP or database call a tool made |

The baseline is almost always one model provider request and zero tool-side calls. That is the comparison, not a magic number to assert.

## Verdict

Accuracy is why the harness exists. The other three axes are the price.

Confirm budgets with the user before treating a delta as pass or fail. Default is to report the four deltas. Do not invent thresholds.

- The harness **wins** when it is more accurate, and any extra latency, tokens, or network requests sit inside a budget the user accepted.
- Tied accuracy: the cheaper, faster, fewer-request side wins.
- A harness that is not more accurate has lost, whatever it cost.

## How to write one

1. Name the task. Confirm the model id both sides will use, and the budgets (or "report deltas, no budget").
2. Write one case: the input, and an expected answer from an independent source of truth (a spec line, a worked example, a known-good literal).
3. Run both sides, or write a runner that does. Same model id. Tools on for the harness, tools off for the baseline.
4. Record four numbers per side and the four deltas.
5. Next case. One case at a time: each eval is a tracer bullet that responds to what the last run taught you.

Match the repo's existing test or eval runner if it has one. If it does not, a small script that prints the four deltas is enough. Do not add a framework to write four numbers. Run the real model on both sides.

## Anti-patterns

- **Unpaired**: scoring the harness with no baseline run.
- **Tautological judge**: the same model grades its own output, or the rubric is in the generating prompt. Expected answers come from outside the model.
- **Accuracy-only**: dropping latency, tokens, or network requests.
- **Wrong model**: the baseline uses a different model than the harness wraps.
- **Horizontal slicing**: a batch of imagined cases written before any paired run.
