---
number: 13190
title: "`uv pip` misinterprets `.devN` versions as equal to the base release version"
type: issue
state: closed
author: crasylph
labels:
  - question
assignees: []
created_at: 2025-04-29T08:43:52Z
updated_at: 2025-04-29T14:34:59Z
url: https://github.com/astral-sh/uv/issues/13190
synced_at: 2026-01-10T01:25:29Z
---

# `uv pip` misinterprets `.devN` versions as equal to the base release version

---

_Issue opened by @crasylph on 2025-04-29 08:43_

### Summary

It seems that `uv pip` does not correctly implement [PEP 440](https://peps.python.org/pep-0440/) rules for pre-releases. Specifically, versions like `0.0.2.dev3` are treated as equal to `0.0.2`, instead of being strictly less than `0.0.2`.

```bash
uv pip install "test_package==0.0.2.dev3,<0.0.2"   # fails ❌
uv pip install "test_package==0.0.2,<0.0.2.dev3"   # fails ❌
```

In both cases, `uv pip` reports:
```bash
  × No solution found when resolving dependencies:
  ╰─▶ you require test_package ∅
```

Expected behavior (per PEP 440)
- `0.0.2.dev3` < `0.0.2`
- `0.0.2` should not match `<0.0.2.dev3`

Is it a bug or I misunderstood anything?

### Platform

Ubuntu20.04 amd64

### Version

uv 0.6.14

### Python version

Python 3.13.3

---

_Label `bug` added by @crasylph on 2025-04-29 08:43_

---

_Label `bug` removed by @konstin on 2025-04-29 12:53_

---

_Label `question` added by @konstin on 2025-04-29 12:53_

---

_Comment by @konstin on 2025-04-29 12:54_

While PEP 440 versions are strictly ordered, the operators such as `<` do not necessarily use that ordering. Instead, they have special rules for pre- and postreleases. For the case of less than and prereleases, PEP 440 says:

> The exclusive ordered comparison <V MUST NOT allow a pre-release of the specified version unless the specified version is itself a pre-release. Allowing pre-releases that are earlier than, but not equal to a specific pre-release may be accomplished by using <V.rc1 or similar.

That avoids `>1,<2` from including `2.0.0a1`, which is generally not compatible with the 1.x series, but has breaking changes from the 2.x series.

---

_Closed by @konstin on 2025-04-29 12:55_

---

_Comment by @notatallshaw on 2025-04-29 14:34_

@konstin is correct, in the PEP 440 spec the exclusive comparison specifier `<V` does not preserve ordinality and is not transative. 

You should use `<=V` if you want a comparison specifier you can reason about with these common properties you would expect. 

---
