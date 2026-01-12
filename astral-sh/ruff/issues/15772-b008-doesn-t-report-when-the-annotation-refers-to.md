```yaml
number: 15772
title: "`B008` doesn't report when the annotation refers to an immutable type, but `RUF009` does"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-01-27T16:23:52Z
updated_at: 2025-02-10T08:46:24Z
url: https://github.com/astral-sh/ruff/issues/15772
synced_at: 2026-01-12T15:54:54Z
```

# `B008` doesn't report when the annotation refers to an immutable type, but `RUF009` does

---

_@InSyncWithFoo_

Minimal reproducible example ([playground](https://play.ruff.rs/d559f6aa-7617-4e99-97a0-57198b791c5f)):

```python
def _(
    this_is_fine: int = f(),           # No error
    this_is_not: list[int] = f()       # B008: Do not perform function call `f` in argument defaults
): ...


@dataclass
class _:
    this_is_not_fine: list[int] = f()  # RUF009: Do not perform function call `f` in dataclass defaults
    this_is_also_not: int = f()        # RUF009: Do not perform function call `f` in dataclass defaults
```

[`RUF009`](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/) should behave similar to [`B008`](https://docs.astral.sh/ruff/rules/function-call-in-default-argument/) and ignore attributes with immutable types.

---

_Comment by @InSyncWithFoo on 2025-01-27 16:24_

This should make for a good first issue.

---

_Comment by @dhruvmanila on 2025-01-29 10:07_

Yeah, this seems reasonable. I think it's because we don't look at the annotation which can be done using `is_immutable_annotation`.

---

_Label `bug` added by @dhruvmanila on 2025-01-29 10:07_

---

_Label `good first issue` added by @dhruvmanila on 2025-01-29 10:07_

---

_Comment by @smokyabdulrahman on 2025-02-08 23:39_

I hope [this PR](https://github.com/astral-sh/ruff/pull/16048) resolved the issue.

---

_Closed by @MichaReiser on 2025-02-10 08:46_

---

_Closed by @MichaReiser on 2025-02-10 08:46_

---
