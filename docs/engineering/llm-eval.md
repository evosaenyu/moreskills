## What it does

`llm-eval` writes evals that compare a [harness](https://www.aihero.dev/ai-coding-dictionary/harness) to the [model](https://www.aihero.dev/ai-coding-dictionary/model) it wraps. The same task runs twice: once with tools, skills, and the agent loop, once as the **baseline** (same model, no tools). The score that matters is the delta on four axes: latency, tokens, accuracy, and network requests.

It never scores a harness in isolation. A number with no baseline is not an eval. That is the whole constraint: you are measuring whether the scaffolding earns its keep against the model you already pay for, not whether the harness can produce an answer.

## When to reach for it

Type `/llm-eval`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it automatically when a task fits: evals of an agent, a [tool](https://www.aihero.dev/ai-coding-dictionary/tool) loop, or an LLM wrapper, or a question about whether the harness beats the model on latency, [tokens](https://www.aihero.dev/ai-coding-dictionary/token), accuracy, or network requests.

| Your situation | Where to go |
| --- | --- |
| The behaviour is a harness, an agent, a tool loop, an LLM wrapper | `llm-eval` |
| The behaviour is ordinary code with defined inputs and outputs | [tdd](https://aihero.dev/skills-tdd) |
| You are hunting a live performance regression, not writing an eval | [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs) |
| The question is whether this approach works at all, in throwaway code | [prototype](https://aihero.dev/skills-prototype) |

## The bakeoff

Three words carry this skill.

**Eval.** A paired run, not a test. Tests are binary at a seam. An eval is graded, and it is graded against a baseline, not against zero.

**Baseline.** The same model id, tools off. Almost always one [model provider request](https://www.aihero.dev/ai-coding-dictionary/model-provider-request) and zero tool-side calls. If the baseline uses a different model, you are comparing models, not measuring the harness.

**Four axes.** Accuracy is why the harness exists. Latency, tokens, and network requests are the price. Network requests count every outbound round trip: each model provider request, plus each HTTP or database call a tool made. A harness that matches baseline accuracy at three times the tokens has lost. A harness that is more accurate inside a budget you accepted has won. Confirm those budgets before treating a delta as pass or fail; the default is to report the four numbers, not to invent thresholds.

One case at a time. A batch of imagined evals written before any paired run is the same failure [tdd](https://aihero.dev/skills-tdd) calls horizontal slicing: you test the shape of the harness rather than a real miss.

## Common questions

**Why not just use `/tdd`?**

`tdd` is a binary loop at a pre-agreed seam. It treats call counts as a red flag, because those assertions couple to internals. An eval *is* the how: token usage, latency, and total network requests are the contract. Putting that inside `tdd` gives the agent two contradictory rules in one file. Reach for `llm-eval` when the thing under test is a harness; reach for `tdd` when it is a module.

**Does the harness have to win on every axis?**

No. Accuracy is the reason it exists; the other three are the price. Tied accuracy, and the cheaper, faster, fewer-request side wins. A harness that is not more accurate has lost, whatever it cost.

## It's working if

- Every case prints two columns of numbers (harness and baseline), not one.
- The expected answer is a literal you can trace to a spec or a worked example, not "whatever the model said".
- A harness that matches baseline accuracy at much higher token or request cost is reported as a loss.
- The network-request count is present, including tool-side HTTP or database calls, not only "it got the right answer".
- The baseline run used the same model id as the harness, with tools off.

## Where it fits

A reach-for-it-anytime standalone, the eval counterpart of [tdd](https://aihero.dev/skills-tdd). It is not a step in the main chain: [implement](https://aihero.dev/skills-implement) still drives `tdd` for ordinary code. `tdd` points here when the behaviour under test is a harness. When you are unsure which skill fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
