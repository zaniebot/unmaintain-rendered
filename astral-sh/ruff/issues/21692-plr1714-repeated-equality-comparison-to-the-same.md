```yaml
number: 21692
title: "PLR1714: Repeated equality comparison to the same literal value shouldn't convert to a set"
type: issue
state: closed
author: injust
labels:
  - rule
assignees: []
created_at: 2025-11-29T10:27:57Z
updated_at: 2026-01-02T18:03:19Z
url: https://github.com/astral-sh/ruff/issues/21692
synced_at: 2026-01-12T15:54:57Z
```

# PLR1714: Repeated equality comparison to the same literal value shouldn't convert to a set

---

_@injust_

### Summary

Observed this in and split it out of https://github.com/astral-sh/ruff/issues/21690.

```python
def test(foo: str) -> None:
    if foo == "bar" or foo == "bar":
        pass
```

https://play.ruff.rs/40871aec-3cf3-48fd-b42f-76f1b8c4118f

PLR1714 changes this to:

```python
def test(foo: str) -> None:
    if foo in {"bar", "bar"}:
        pass
```

~~If the values being compared are literals, the equality comparisons should be merged instead of converting to a set.~~

---

_Comment by @MichaReiser on 2025-11-30 11:53_

I'm leaning towards not flagging `PLR1714` if all members are equal because fixing it to a single equal isn't what the rule is testing for. Detecting that the second equal is unnecessary is something that falls into a generic unreachable rule

---

_Label `rule` added by @MichaReiser on 2025-11-30 11:54_

---

_Closed by @ntBre on 2026-01-02 17:56_

---
