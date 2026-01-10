---
number: 20350
title: PERF401 rule confusion
type: issue
state: closed
author: minusf
labels:
  - question
assignees: []
created_at: 2025-09-11T14:44:49Z
updated_at: 2025-09-15T14:04:32Z
url: https://github.com/astral-sh/ruff/issues/20350
synced_at: 2026-01-10T01:23:01Z
---

# PERF401 rule confusion

---

_Issue opened by @minusf on 2025-09-11 14:44_

### Summary

I might be confused, but it seems to me that the `PERF401` rule gives misleading advice in certain valid use cases, for example while generating a `select` widget in django (a list of tuples):


```py
select_choices = [("", "--- Empty ---")]

for user in users:
    if some_condition:
        select_choices.append((user["this"], user["that"]))
        # Use `list.extend` to create a transformed list (PERF401)
```

https://docs.astral.sh/ruff/rules/manual-list-comprehension/ says:

> If you're appending to an existing list, use the extend method instead:

But this is not universal advice is it?  `extend` flattens, while `append` does not. If ruff only looks at the argument of `append` and sees an iterable, it does not mean automatically that `extend` can be used...

I ran the original `perflint` on this code and it is still advising to replace the whole `for` loop. Maybe the check is simplistic, or the code really should be rewritten in another way, but the for loop appends to an already existing list with actual values in it... Seems like a false positive to me...

---

_Renamed from "PERF401" to "PERF401 rule confusion" by @minusf on 2025-09-11 14:45_

---

_Comment by @ntBre on 2025-09-11 16:09_

If you enable `--preview` mode and apply the suggested fix ([playground](https://play.ruff.rs/b3d8433b-ee26-4d07-8c20-301dda6b9a83)), you'll see that the code Ruff fixes this example to is this:

```py
select_choices = [("", "--- Empty ---")]

select_choices.extend((user["this"], user["that"]) for user in users if some_condition)
```

So it does replace the whole `for` loop with code that should be equivalent. I think that corresponds to the second `use instead` example from the rule docs too.

I can see how the short diagnostic message is misleading, but the rule isn't trying to say "switch from `append` to `extend`," it means to apply the whole rule transformation, resulting in a generator argument to `extend`.

---

_Label `question` added by @ntBre on 2025-09-11 16:09_

---

_Comment by @minusf on 2025-09-11 17:09_

Thank you for the explanaiton, I could have tried the proposed fix to make the suggestion clearer for myself.

Is there a "cutoff" point where this suggestion is dropped, for example when the generator expression would become really "ugly" and difficult to read (e.g. the condition gets long and/or the item object to extend/append gets also very long)? Or is it always generators for the win, no matter what? 😄  

---

_Comment by @minusf on 2025-09-11 17:18_

Actually I have found some of my own code where the generator tip, even the extend tip instead of append does not show up anymore. It just needs enough variables 😄 

---

_Comment by @ntBre on 2025-09-11 18:18_

Ah interesting, I would have said "no" there's no cutoff 😆 so now I'm curious where it stopped. The rule as a whole only applies to simple `for` loops with either a single non-`if` statement or a single `if` with a single nested statement (among other restrictions), so you may have hit one of those.

---

_Closed by @MichaReiser on 2025-09-15 14:04_

---
