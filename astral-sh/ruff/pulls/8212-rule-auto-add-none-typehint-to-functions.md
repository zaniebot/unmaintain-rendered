```yaml
number: 8212
title: "Rule: Auto-add `-> None` typehint to functions"
type: pull_request
state: closed
author: intgr
labels: []
assignees: []
draft: true
base: main
head: suggest-typehint-return-None
created_at: 2023-10-25T12:22:31Z
updated_at: 2023-11-14T07:15:01Z
url: https://github.com/astral-sh/ruff/pull/8212
synced_at: 2026-01-10T23:40:55Z
```

# Rule: Auto-add `-> None` typehint to functions

---

_Pull request opened by @intgr on 2023-10-25 12:22_

## Summary

**WIP:** This is a very early partial draft still. No need to do serious review yet, but feel free to give me pointers.

I want to create a new Ruff rule that auto-adds `-> None` return type hint to functions when no `return <value>` statements are present in the function.

Clearly there are lots of special cases I need to handle (`yield`, `raise`, `@overload`, `.pyi` files, etc).

I currently used rule code `RUF300`, which is unprecedented. But I think it may make sense to allocate a specific RUF range for typehinting-related rules. I have a few more ideas similar to this one.

* Related issue: #8213

## Test Plan

TBD.


---

_Renamed from "Rule: Auto-add return None typehint to functions" to "Rule: Auto-add `-> None` typehint to functions" by @intgr on 2023-10-25 12:28_

---

_Comment by @intgr on 2023-10-25 12:33_

Also some relevant points here that I should consider:

* https://github.com/astral-sh/ruff/issues/3704


---

_Comment by @diceroll123 on 2023-10-25 17:11_

Might need to special case `NoReturn`, not limited to the following:

![image](https://github.com/astral-sh/ruff/assets/1243957/5d2d785d-0488-4d9a-8361-41877b38997c)


---

_Comment by @charliermarsh on 2023-10-25 17:18_

I haven't had a chance to review the code yet, but I think we could just treat this as a sometimes-available autofix for [`ANN201`](https://docs.astral.sh/ruff/rules/missing-return-type-undocumented-public-function/) and the other rules that check for return type annotations, rather than treating it as a separate rule.

---

_Comment by @intgr on 2023-10-25 17:47_

> haven't had a chance to review the code yet

Just be aware that it's very incomplete and broken right now.

> we could just treat this as a sometimes-available autofix for [ANN201](https://docs.astral.sh/ruff/rules/missing-return-type-undocumented-public-function/) and the other rules that check for return type annotations, rather than treating it as a separate rule

I see where you're coming from. My initial thoughts:

* I imagine that this rule doesn't just *add* a hint, but can *change* a hint that it determines is incorrect. But it remains to be seen if this can be done reliably ([#3704](https://github.com/astral-sh/ruff/issues/3704) is a counterexample).
* If we add several ways to infer return types, some of them more reliable than others, then by grouping them under one rule, users lose the ability to pick which ones they want and which they don't. But perhaps the distinction between safe/unsafe edits is sufficient here.

But I'm by no means committed to this approach.


---

_Comment by @charliermarsh on 2023-11-13 04:46_

I ended up adding a similar fix for the ANN rules as a fun Sunday-night hack: https://github.com/astral-sh/ruff/pull/8643. It won't apply to functions that already have annotations as you mention, but the logic does extend to other primitive types and unions of those types.

---

_Closed by @intgr on 2023-11-14 07:15_

---
