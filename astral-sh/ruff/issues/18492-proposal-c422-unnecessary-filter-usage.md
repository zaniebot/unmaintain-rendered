```yaml
number: 18492
title: "Proposal: C422 Unnecessary filter() usage"
type: issue
state: open
author: grihabor
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-06T09:36:40Z
updated_at: 2025-11-27T15:12:00Z
url: https://github.com/astral-sh/ruff/issues/18492
synced_at: 2026-01-12T15:54:56Z
```

# Proposal: C422 Unnecessary filter() usage

---

_@grihabor_

### Summary

## What it does

Checks for unnecessary `filter()` calls with lambda functions.

Why is this bad?

Using `filter(func, iterable)` when func is a lambda is slower than using a generator expression or a comprehension, as the latter approach avoids the function call overhead, in addition to being more readable.

This rule also applies to `filter()` calls within `list()`, `set()`, and `dict()` calls. For example:

- Instead of `list(filter(lambda num: num < 10, nums))`, use `[num for num in nums if num < 10]`.
- Instead of `set(filter(lambda num: num % 2 == 0, nums))`, use `{num for num in nums if num % 2 == 0}`.
- Instead of `dict(filter(lambda pair: pair[1] is not None, items))`, use `{k: v for k, v in values if v is not None}`.

## Example
```python
filter(lambda x: x % 2 == 0, iterable)
```
Use instead:
```python
(x for x in iterable if x % 2 == 0)
```

---

_Comment by @ntBre on 2025-06-06 16:32_

Is this rule present in the upstream flake8-comprehensions? I checked the [PyPI page](https://pypi.org/project/flake8-comprehensions/), and it looked like it stopped at C420. We could still consider the rule, but we'd want to be careful adding it as a `C` rule without an upstream feature request.

---

_Label `rule` added by @ntBre on 2025-06-06 16:32_

---

_Label `needs-decision` added by @ntBre on 2025-06-06 16:32_

---

_Comment by @grihabor on 2025-06-07 00:08_

No, it's not in upstream, but it fits the overall theme of C4 checks. Should I open an proposal in upstream? Or should we choose a different rule tag?

---

_Comment by @ntBre on 2025-06-07 00:19_

I think it may be okay in light of [this comment](https://github.com/astral-sh/ruff/issues/18331#issuecomment-2922513633) from the flake8-comprehensions maintainer last week. I forgot that was for the same linter.

---

_Renamed from "Proposal: C421 Unnecessary filter() usage" to "Proposal: C422 Unnecessary filter() usage" by @grihabor on 2025-06-07 08:48_

---

_Comment by @grihabor on 2025-06-07 08:49_

Ok, so C421 is taken, let's reserve C422 then.

---

_Comment by @grihabor on 2025-06-07 08:55_

Hey @adamchainz, do you have any thoughts on this rule?

---

_Comment by @SpecLad on 2025-11-27 15:12_

FWIW, I had filed an upstream feature request for this: adamchainz/flake8-comprehensions#622.

---
