---
number: 4858
title: "UP007: opt out of Optional"
type: issue
state: closed
author: Fogapod
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-06-05T07:18:42Z
updated_at: 2025-02-19T11:28:19Z
url: https://github.com/astral-sh/ruff/issues/4858
synced_at: 2026-01-07T13:12:14-06:00
---

# UP007: opt out of Optional

---

_Issue opened by @Fogapod on 2023-06-05 07:18_

Hello, currently UP007 covers both `Union[a, b, c]` -> `a | b | c` and `Optional[a]` -> `a | None`.
Personally i find Optional to be more readable and PEP 604 does not deprecate this syntax either.

I'm not sure how it could be changed without breaking changes. I see 2 ways:

- `UP007` stops reporting Optional, keeps reporting Union and later added rules
- `UP039` is added which only reports Optional

The downside of this is that it forces people to update config to get previous behavior if they didn't have entire `UP` category enabled. Alternative is:

- `UP007` remains unchanged
- `UP039` is added which reports Union and later added rules

This might be even worse because
1) for people with `UP` in config it does double work
2) if someone had `UP` enabled and `UP007` ignored, the Union lint would reappear (although I'd assume most people who do this because of Optional)

pyupgrade author made it clear in multiple issues that they are not going to change this https://github.com/asottile/pyupgrade/issues/390#issuecomment-1046896890. don't know how much you are willing to deviate from "upstream" for cases like this but it would be a nice addition 

---

_Label `question` added by @charliermarsh on 2023-06-08 19:57_

---

_Comment by @charliermarsh on 2023-06-09 15:36_

A third option would be to add a setting to retain `Optional`, but keep all behavior under `UP007`. I'm somewhat undecided on this overall though.

---

_Comment by @lengau on 2023-06-27 22:15_

Just adding my voice to agree with this.

It appears that most people are either in favour of or indifferent to `X | Y` replacing `Union[X, Y]`, but a big chunk (roughly half) of those prefer to disable `UP007` to keep `Optional[Z]` over `Z | None`. Personally I'm indifferent to the latter and have a moderate preference for `X | Y`, so I'd prefer to be able to enforce the replacement of `Union` without enforcing the replacement of `Optional`, allowing the use of `Optional`.

Breaking it up into two rules (or perhaps even three, leaving `UP007` as the union of the two rules with an eventual deprecation, much like with #1980) is probably more intuitive than having it be a configuration on the rule. Despite `Optional[X]` [literally being](https://github.com/python/cpython/blob/3.11/Lib/typing.py#L706) the same as `Union[X, None]` from a logical standpoint, from a readability and utility standpoint they are quite different (`Optional` having a special enough meaning to have been distinguished from `Union` in the first place). If there were a need to match upstream rules like in Pycodestyle I'd probably have the opposite opinion, but as pyupgrade bundles its rules by Python version a direct mapping is already out of the window (e.g. UP031 and UP032 being separate already).

---

_Comment by @charliermarsh on 2023-06-29 00:18_

I personally don't have a preference for `Optional[a]` over `a | None` but I'm not opposed to splitting these into separate rules. (I'd prefer separate rules over a setting.) My hesitation is that I'm not sure how to do that migration in a way that's minimally disruptive.

> ...but a big chunk (roughly half)...

Not challenging this, but is this based on data? Like a survey or something? Or your own experiences and interactions?


---

_Comment by @lengau on 2023-06-30 20:22_

Not based on a formal survey, just based on asking other developers what they think

---

_Comment by @lengau on 2023-06-30 20:33_

What if UP007 became a way to just enable both of the split out rules + write a deprecation warning?

I'm not too worried about the edge case where someone has `UP` enabled but ignores `UP007` specifically, as updates right now are well known to introduce new rules that will introduce failures, so nobody should be surprised by it (and the command output and documentation are both good enough to make it easy for people to understand and update as needed). If you are concerned about that edge case, you could treat having the ignore group include UP007 disable the other rules.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:12_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:12_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:12_

---

_Referenced in [spear-ai/citizen#336](../../spear-ai/citizen/issues/336.md) on 2023-12-19 18:19_

---

_Comment by @ktbarrett on 2024-03-12 03:37_

Whose decision is this waiting on? I think the first solution is the least disruptive and I would also like to see this rule split between `Union` and `Optional` as I *much* prefer `Optional`: it's clearer documentation that the argument is optional to those not in the know. I'd also would ***not*** like to see recommendations in the other direction (to prefer `Optional` over `X | None`).

In my opinion `Optional[X] = None` and `X | None` (no default) encode two different idioms and are the best ways to spell those idioms.

---

_Comment by @zanieb on 2024-03-12 05:58_

Sure. Someone can do this if they want:

1. Drop `Optional` support from `UP007` in preview mode
2. Add new rule in preview for use of `Optional`
3. Add documentation about the transition to both rules

I'm not too concerned about people ignoring `UP007` right now encountering the new rule, if anything that will be a signal that they can enable `UP007` again. It'll happen in a breaking release and we'll be able to gauge how much effect it will have when 2. is implemented.

---

_Label `needs-decision` removed by @zanieb on 2024-03-12 05:58_

---

_Label `help wanted` added by @zanieb on 2024-03-12 05:58_

---

_Referenced in [astral-sh/ruff#11379](../../astral-sh/ruff/pulls/11379.md) on 2024-05-12 11:43_

---

_Comment by @JelleZijlstra on 2024-05-14 14:11_

> it's clearer documentation that the argument is optional to those not in the know

But it isn't: Optional means the parameter may be None, not that it is optional. A parameter can be optional (i.e., have a default) without having an Optional type, and vice versa. This is actually one of the main reasons from my perspective why Optional *should* be avoided: its name is misleading.

---

_Comment by @ktbarrett on 2024-05-14 19:56_

Perhaps `Optional` *shouldn't* mean the same thing as `X | None`, but instead pass X, or don't give an argument, giving a warning otherwise. Not that I expect this to ever change... but explicitly passing None to optional arguments (which apparently has no spelling at all) is a code smell.

---

_Referenced in [edwardzjl/chatbot#527](../../edwardzjl/chatbot/pulls/527.md) on 2024-07-11 09:11_

---

_Referenced in [oerc0122/castep_outputs#163](../../oerc0122/castep_outputs/pulls/163.md) on 2024-09-06 08:44_

---

_Referenced in [astral-sh/ruff#15313](../../astral-sh/ruff/pulls/15313.md) on 2025-01-07 06:20_

---

_Closed by @MichaReiser on 2025-01-07 10:23_

---

_Comment by @randolf-scholz on 2025-02-19 11:28_

> But it isn't: Optional means the parameter may be None, not that it is optional. A parameter can be optional (i.e., have a default) without having an Optional type, and vice versa. 

Clearly this is the opportunity for a new linting rule, enforcing a consistent style:

1. When `Optional[T]` is used as an annotation for a function parameter or class attribute, it must have a default value
2. For parameters/variables without a default value, use `T | None` instead.


---

_Referenced in [microsoft/pyright#9793](../../microsoft/pyright/issues/9793.md) on 2025-03-18 19:20_

---
