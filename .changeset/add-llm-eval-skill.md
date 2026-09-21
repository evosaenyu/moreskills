---
"mattpocock-skills": minor
---

Add **`llm-eval`**, a model-invoked engineering skill for evaluating a harness against the same model with no tools. Every eval is a paired run scored on latency, tokens, accuracy, and network requests; the score that matters is the delta, not the harness's number in isolation. Wired as a promoted skill (plugin entry, top-level + Engineering READMEs, docs page) with a neighbour pointer from `tdd` and a Standalone route in `ask-matt`.
