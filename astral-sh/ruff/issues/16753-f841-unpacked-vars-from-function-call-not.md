```yaml
number: 16753
title: F841 unpacked vars from function call not reported as unused
type: issue
state: closed
author: sk-
labels:
  - question
assignees: []
created_at: 2025-03-14T16:01:41Z
updated_at: 2025-03-14T17:15:04Z
url: https://github.com/astral-sh/ruff/issues/16753
synced_at: 2026-01-10T11:09:57Z
```

# F841 unpacked vars from function call not reported as unused

---

_Issue opened by @sk- on 2025-03-14 16:01_

### Summary

F841 is not reporting as unused unpacked values from function calls.

Below is some code showcasing the issue ([playground](https://play.ruff.rs/23a484d2-fe0e-4135-ab8c-32a83fea5a94)):

```python
def func(x):
    t1 = tuple([1, 2])
    a1, b1 = tuple([1, 2])

    t2 = (1, 2)
    a2, b2 = (1, 2)
```

In the code above `a1` and `b1` are not reported as unused, whereas `a2` and `b2` are correctly reported as unused.

```
Local variable `t1` is assigned to but never used (F841) [Ln 2, Col 5]
Local variable `t2` is assigned to but never used (F841) [Ln 5, Col 5]
Local variable `a2` is assigned to but never used (F841) [Ln 6, Col 5]
Local variable `b2` is assigned to but never used (F841) [Ln 6, Col 9]
```

### Version

v0.11.0 (playground)

---

_Comment by @ntBre on 2025-03-14 16:58_

This is covered by the [`unused-unpacked-variable`](https://docs.astral.sh/ruff/rules/unused-unpacked-variable/) (`RUF059`) preview rule.

This is a recent addition. You can follow some of the discussion about splitting it out [here](https://github.com/astral-sh/ruff/issues/8884) if you're curious.

---

_Label `question` added by @ntBre on 2025-03-14 16:58_

---

_Closed by @sk- on 2025-03-14 17:15_

---
