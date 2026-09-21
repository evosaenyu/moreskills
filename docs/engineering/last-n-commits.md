## What it does

`last-n-commits` **loads** the diffs of the last n commits into the current [context window](https://www.aihero.dev/ai-coding-dictionary/context-window). Once the diffs are in, it stops.

The diffs stay in the tool output. You get a one-line confirmation and the oneline commit list, not a recap of what the commits did. The [agent](https://www.aihero.dev/ai-coding-dictionary/agent) does not review, summarise, or interpret. You asked for the diffs themselves, which are the [primary source](https://www.aihero.dev/ai-coding-dictionary/primary-source); a summary would replace them with a second-hand account and spend the [tokens](https://www.aihero.dev/ai-coding-dictionary/token) twice.

## When to reach for it

You invoke this by typing `/last-n-commits n`, and the agent won't reach for it on its own. n is yours to pick: loading diffs is a [context](https://www.aihero.dev/ai-coding-dictionary/context) budget decision.

| Your situation | Reach for |
| --- | --- |
| You want the last n commit diffs sitting in this [session](https://www.aihero.dev/ai-coding-dictionary/session), then you will decide | `last-n-commits` |
| You want a Standards and Spec review of a diff since a fixed point | [code-review](https://aihero.dev/skills-code-review) |
| Git has already stopped on merge or rebase conflicts | [resolving-merge-conflicts](https://aihero.dev/skills-resolving-merge-conflicts) |

## Load, then stop

The leading word is **load**. The default move, without this skill, is to run `git log`, read it, and tell you what changed. That recap is not the diffs. This skill exists so the patches themselves occupy the window, and so the run ends there instead of turning into a review you did not ask for.

Pass n as the argument: `/last-n-commits 5`. If you omit it, the skill asks rather than guessing. If history is shorter than n, it says so and loads what exists. Uncommitted work is out of scope; this is commits only.

A large n can fill the window. That is why the skill is user-invoked: you choose how much history to spend.

## Common questions

**How is this different from `/code-review`?**

`code-review` judges a diff since a fixed point along two axes. This skill only loads. If you want findings, use `code-review`. If you want the patches in context so you can work from them, use this.

**Does it include uncommitted work?**

No. It is `git log` of the last n commits. Staged and working-tree changes are invisible to it. Commit first if that work is what you wanted loaded.

## It's working if

- The reply is a one-line confirmation plus the oneline hashes, not a recap of what the commits did.
- The tool trace shows `git log -n <n> -p`, or `git show` follow-ups if the first output was truncated.
- Invoking without n gets a question, not a guessed number.
- Uncommitted files are left untouched.

## Where it fits

A reach-for-it-anytime standalone. It is not a step in the main chain.

- [code-review](https://aihero.dev/skills-code-review) is the closest neighbour: both look at git diffs, this one loads them, `code-review` judges them.
- [resolving-merge-conflicts](https://aihero.dev/skills-resolving-merge-conflicts) is the other git standalone: it is for an in-progress conflict, not for reading recent history.

[ask-matt](https://aihero.dev/skills-ask-matt) routes across the whole set when you are unsure which skill the situation wants.
