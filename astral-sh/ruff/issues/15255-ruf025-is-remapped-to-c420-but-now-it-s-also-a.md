---
number: 15255
title: "RUF025 is remapped to C420 but now it's also a new rule"
type: issue
state: closed
author: oprypin
labels:
  - bug
assignees: []
created_at: 2025-01-04T12:52:05Z
updated_at: 2025-01-05T08:49:16Z
url: https://github.com/astral-sh/ruff/issues/15255
synced_at: 2026-01-10T01:22:56Z
---

# RUF025 is remapped to C420 but now it's also a new rule

---

_Issue opened by @oprypin on 2025-01-04 12:52_

&bull; https://github.com/astral-sh/ruff/pull/12533 remapped the rule `RUF025`  as `C420`.

&bull; https://github.com/astral-sh/ruff/pull/15104 added a different new rule under the code `RUF025`.

The immediate issue is that even though RUF025 has just appeared [in documentation](https://docs.astral.sh/ruff/rules/unnecessary-empty-iterable-within-deque-call/), if you enable it, the rule doesn't actually work, and you just get the following warning:
```
warning: `RUF025` has been remapped to `C420`.
```

So I suppose (luckily?) the new rule was not *actually* added yet because it's impossible to use it, it only appears in docs and in the codebase.

Rule codes that were remapped from are supposed to remain reserved forever, right?

---

_Referenced in [astral-sh/ruff#15104](../../astral-sh/ruff/pulls/15104.md) on 2025-01-04 12:52_

---

_Comment by @MichaReiser on 2025-01-04 13:14_

> Rule codes that were remapped from are supposed to remain reserved forever, right?


Argh, thanks. This is such a foot gun, especially with the many holes in the Ruff code section. I assumed that the redirects are at least visible in the `codes` section because it's the case for deprecated rules. I'll look into this on Monday, but we should have a static check that asserts that `codes` and redirects don't overlap. 

---

_Assigned to @MichaReiser by @MichaReiser on 2025-01-04 13:14_

---

_Label `bug` added by @MichaReiser on 2025-01-04 13:14_

---

_Comment by @InSyncWithFoo on 2025-01-04 14:12_

Current `RUF` "holes":

* [`RUF004`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF004): Redirects to `B026` (#3290, #3293)
* [`RUF014`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF014): Would be a duplicate of `PLW0101` (#5384, #10891)
* [`RUF037`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF037): One rejected (#14111)
* [`RUF042`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF042): One rejected (#14564) and one pending (#14831)
* [`RUF044`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF044): One pending (#14688)
* [`RUF045`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF045): One pending (#14349)
* [`RUF047`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF047): One rejected (#14746) and one pending (#15051)
* [`RUF049`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF049): Empty
* [`RUF050`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF050): Two rejected (#14607, #14763)
* [`RUF053`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF053): Empty
* [`RUF054`](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20RUF054): Empty

It should perhaps be emphasized that I opened most of the aforementioned PRs...

---

_Comment by @MichaReiser on 2025-01-05 08:49_

> So I suppose (luckily?) the new rule was not actually added yet because it's impossible to use it, it only appears in docs and in the codebase.

I think it was possible to use the rule using the `ALL` selector but yes, most users weren't able to use it. 

Thank you @InSyncWithFoo for volunteering to fix this issue so quickly! I also want to make clear that this was entirely my fault and by no means yours, just because we skipped a few rule codes. 

---

_Closed by @MichaReiser on 2025-01-05 08:49_

---
