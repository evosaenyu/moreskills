---
name: last-n-commits
description: Put the diffs of the last n commits into this context.
disable-model-invocation: true
argument-hint: "How many commits?"
---

**Load** the diffs of the last **n** commits into this context. Stop once they are in.

1. Take **n** from the argument (a positive integer). If it is missing or not a positive integer, ask for it and stop.
2. Run `git log -n <n> --oneline`. If the history is shorter than n, say so and continue with what exists.
3. Run `git log -n <n> -p`. If the harness truncates the output, `git show` each missing commit until every diff is in this context.
4. Confirm with one line plus the oneline list from step 2. The diffs stay in the tool output; they are already in context.
