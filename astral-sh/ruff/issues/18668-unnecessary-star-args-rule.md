---
number: 18668
title: unnecessary_star_args rule
type: issue
state: open
author: Andrej730
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-13T19:27:23Z
updated_at: 2025-06-18T05:46:31Z
url: https://github.com/astral-sh/ruff/issues/18668
synced_at: 2026-01-10T01:22:59Z
---

# unnecessary_star_args rule

---

_Issue opened by @Andrej730 on 2025-06-13 19:27_

### Summary

Currently there is PIE804 for unnecessary kwargs - https://docs.astral.sh/ruff/rules/unnecessary-dict-kwargs/

For completeness, would be nice to have a similar rule for `*args`, the benefits are the same - avoiding constructing unnecessary container and more type safety.

```python
def test(a: int, b: int): ...

# Static: not typing errors.
# Runtime: TypeError: test() takes 2 positional arguments but 3 were given.
test(*[5, 4, 23])
```

---

_Label `rule` added by @ntBre on 2025-06-13 20:01_

---

_Label `needs-decision` added by @ntBre on 2025-06-13 20:01_

---

_Comment by @ntBre on 2025-06-13 20:05_

Sounds reasonable to me. There's also [PIE800](https://docs.astral.sh/ruff/rules/unnecessary-spread/) for unpacking one dict into another. We could also have a parallel rule for cases like `[1, 2, 3, *[4, 5, 6]]`.

It looks like the upstream [flake8-pie](https://github.com/sbdchd/flake8-pie) is also archived, so it should be safe to add rules in that namespace.

---

_Comment by @Avasam on 2025-06-17 19:29_

Very similar to https://github.com/astral-sh/ruff/issues/10592

---

_Comment by @Andrej730 on 2025-06-18 05:46_

It is similar, but PIE804 and this issue is more about detecting unnecessary constructors in args/kwargs.

---
