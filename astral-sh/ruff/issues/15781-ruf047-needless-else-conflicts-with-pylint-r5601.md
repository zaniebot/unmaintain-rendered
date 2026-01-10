---
number: 15781
title: "\"RUF047\" (needless-else) conflicts with pylint R5601 (confusing-consecutive-elif), Can't suppress the rule inline."
type: issue
state: open
author: IgnatKudriavtsev
labels:
  - bug
  - suppression
assignees: []
created_at: 2025-01-28T08:28:58Z
updated_at: 2025-01-28T16:57:43Z
url: https://github.com/astral-sh/ruff/issues/15781
synced_at: 2026-01-10T01:22:56Z
---

# "RUF047" (needless-else) conflicts with pylint R5601 (confusing-consecutive-elif), Can't suppress the rule inline.

---

_Issue opened by @IgnatKudriavtsev on 2025-01-28 08:28_

### Description
ruff 0.9.3

Ruff preview rule `RUF047 (needless-else)` complains on the code that has an intentionally added empty `else` branch as one of recommended by pylint ways to fix the  rule [confusing-consecutive-elif / R5601](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/confusing-consecutive-elif.html):

> 

> Description:
> 
> Used when an elif statement follows right after an indented block which itself ends with if or elif. It may not be obvious if the elif statement was willingly or mistakenly unindented. Extracting the indented if statement into a separate function might avoid confusion and prevent errors.
> 
> Problematic code:
> ```
> def myfunc(shall_continue: bool, shall_exit: bool):
>     if shall_continue:
>         if input("Are you sure?") == "y":
>             print("Moving on.")
>     elif shall_exit:  # [confusing-consecutive-elif]
>         print("Exiting.")
> ```
> Correct code:
> 
> ```
> # Option 1: add explicit 'else'
> def myfunc(shall_continue: bool, shall_exit: bool):
>     if shall_continue:
>         if input("Are you sure?") == "y":
>             print("Moving on.")
>         else:
>             pass
>     elif shall_exit:
>         print("Exiting.")
> 
> ```
> ...

Currently we need to either suppress ruff or pylint rules for such cases.

Actually, I'm not sure what is the bigger problem here: the fact the ruff complains on the problem or the fact that I can't suppress it locally.

Local suppression of the ruff locally in this case doesn't work because of `RUF100 [*] Unused 'noqa' directive (unused: 'RUF047')`

---

_Comment by @AlexWaygood on 2025-01-28 11:51_

Hmm, so there are two issues here.

The first issue is about `RUF047` conflicting with `R5601`. This isn't about two Ruff rules conflicting. This is about [R5601 in the original pylint linter](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/confusing-consecutive-elif.html) making a conflicting recommendation with one of Ruff's rules.

Ruff doesn't currently have a reimplementation of R5601 in its pylint category. The rule was considered in https://github.com/astral-sh/ruff/pull/10103, but was rejected for now. In general, we _try_ to guarantee that no two Ruff rules will offer conflicting recommendations, but I don't think we make any guarantees about being able to run Ruff alongside other linters.

The second issue is about not being able to suppress `RUF047`. I [can reproduce that in the Ruff playground](https://play.ruff.rs/5092a900-8b18-40fc-956f-b77c388e2398). That looks like a bug to me!

---

_Label `bug` added by @AlexWaygood on 2025-01-28 11:51_

---

_Label `suppression` added by @AlexWaygood on 2025-01-28 11:51_

---

_Comment by @InSyncWithFoo on 2025-01-28 16:57_

This is an edge case, as `else` branches with any kinds of comments, pragma or not, are never reported. Thus, I think it is more preferable to either suppress `R5601` or add a (normal) comment explaining why that empty branch is necessary.

---
