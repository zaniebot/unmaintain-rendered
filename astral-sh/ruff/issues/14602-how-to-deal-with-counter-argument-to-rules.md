```yaml
number: 14602
title: How to deal with counter-argument to rules? Concrete example PLC1901
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2024-11-26T06:19:34Z
updated_at: 2025-01-25T10:55:10Z
url: https://github.com/astral-sh/ruff/issues/14602
synced_at: 2026-01-12T15:54:54Z
```

# How to deal with counter-argument to rules? Concrete example PLC1901

---

_@kaddkaka_

The pylint rule PLC1901 (`compare-to-empty-string`) checks for comparison with empty string which it considers "unnecessary", but I would argue this is not a problem, but rather the reverse!

Example code from [documentation](https://docs.astral.sh/ruff/rules/compare-to-empty-string/):
```py
x: str = ...

if x == "":
    print("x is empty")
``` 

Even if the compared object is known to be a string or not, `x == ""` shows and explain the intent much better than `not x`.

- If type annotation is missing, or or many lines away, the person reading the code will understand that this variable is meant to be a string. `not x` does not give any clue on what type x is supposed to be.

Could there be a section "Why is this *not* bad" in the documentation?

---

_Comment by @kaddkaka on 2024-11-26 06:27_

There has been complaints regarding PLC1901 in particular before: https://github.com/astral-sh/ruff/issues/4282#issuecomment-1538815106 

---

_Comment by @kaddkaka on 2024-11-26 06:31_

Oh, I just found https://github.com/astral-sh/ruff/pull/5264

Is this rule still in "nursery"? Is PLC1901 meant to only be active if explicitly opt-in? I seem to get it just by having select "PL".

Nothing in the rule page documentation hints about this.


---

_Label `question` added by @MichaReiser on 2024-11-26 08:40_

---

_Comment by @MichaReiser on 2024-11-26 08:43_

Hi @kaddkaka 

Thanks for all the research and linking the relevant issues. That's useful.

Yes, the `PLC` rules aren't for everyone. They encode specific conventions that *some* find useful. There are many other rules in Ruff that are of a stylistic nature or are rather opinionated. We suggest you freely disable rules if you don't agree with what they're asserting. 

---

_Comment by @kaddkaka on 2024-11-26 09:19_

@MichaReiser thanks, I agree this is the way to go. In the closed PR it was mentioned that this rule was moved to a "nursery". Does that corresponds to the rules enabled by  `--preview`? 

---

_Comment by @MichaReiser on 2024-11-26 09:22_

Yes, preview replaced the nursery concept and it is to opt-in to more experimental Ruff features and [PLC1901](https://docs.astral.sh/ruff/rules/compare-to-empty-string/) is one such experimental rule

---

_Comment by @kaddkaka on 2024-11-28 06:36_

Would it be valuable to have more rule categories here?

1. rules that are new and not yet battle-tested (they enter in `--preview` and are moved to stable set of rules when mature)
2. Some rules are considered not good and it's not valuable to include them in the "default" set of rules. these rules should perhaps be stable put "opt-in"

So instead of:
- `--preview` and `stable`, have:
- `--preview`, `stable-default`, `stable-opt-in`

---

_Comment by @MichaReiser on 2024-11-28 06:38_

Yes, we think that's the long term solution for this. We plan to recategorize rules but haven't gotten around to actually do it. See https://github.com/astral-sh/ruff/issues/1774

---

_Closed by @MichaReiser on 2025-01-25 10:55_

---
