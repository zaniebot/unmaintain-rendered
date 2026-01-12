```yaml
number: 555
title: Issues detecting unbound variables defined in the loops
type: issue
state: closed
author: Andrej730
labels: []
assignees: []
created_at: 2025-05-30T18:58:08Z
updated_at: 2025-05-31T05:55:15Z
url: https://github.com/astral-sh/ty/issues/555
synced_at: 2026-01-12T15:54:23Z
```

# Issues detecting unbound variables defined in the loops

---

_@Andrej730_

### Summary

Noticed two kind of issues:

1. Possibly unbound variable reported as not defined, not as "possibly unbounded", which may create a bit of confusion.
https://play.ty.dev/99dfb31f-ac48-42cb-ae09-f9409534c4fc
Also could be related to #162

```python
d = [(x := i) for i in range(5)]
reveal_type(d)
# error[unresolved-reference]: Name `x` used when not defined
print(x)
```

2. Same case but with for-loop - it doesn't detect variable neither as possibly unbound / undefined reference
https://play.ty.dev/8c5a457c-02ef-4c3b-91d1-47686ad1a50d
```python
for i in range(5):
    x = i
# All checks passed!
print(x)
```

### Version

ty 0.0.1-alpha.7 (afb20f6fe 2025-05-26)

---

_Comment by @AlexWaygood on 2025-05-30 20:11_

Thanks! Your first issue is indeed a duplicate of #162, as you say.

Your second issue is a totally different topic. We do emit an error on this snippet if you enable our `possibly-unresolved-reference` error code: https://play.ty.dev/b4a24adb-681a-419c-8d08-17f1d1401c54. The error code is currently disabled by default -- you have to opt into it -- because it still has some false positives right now that we're working on fixing. We hope to promote it to enabled-by-default before we release a stable version of ty.

---

_Closed by @AlexWaygood on 2025-05-30 20:11_

---

_Comment by @AlexWaygood on 2025-05-30 20:15_

Unfortunately the nature of `possibly-unresolved-reference` is such the number of false positives may always be too high to have it be enabled by default, however. There are a lot of ways you can write quite dynamic code which this kind of check would struggle to understand. If we had it be enabled by default, it would probably lead to lots of user complaints regarding false positives, sadly.

---

_Comment by @carljm on 2025-05-30 20:16_

Example case that no current type checker understands (and we don't have any current plans to either) where this diagnostic leads to false positives:

```py
if flag:
    a = 1
...
if flag:
    use(a)
```

We provide the diagnostic, so that if you prefer to avoid this kind of code pattern you can opt in to it. But it's disabled by default because we don't want to emit false positives on working code.

---

_Comment by @Andrej730 on 2025-05-31 05:55_

Thank you, here goes the start of my `ty.toml` 😁

---
