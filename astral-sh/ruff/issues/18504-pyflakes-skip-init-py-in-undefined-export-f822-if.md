```yaml
number: 18504
title: "[`pyflakes`] Skip `__init__.py` in `undefined-export` (`F822`) if `__getattr__` is overriden at the module level?"
type: issue
state: open
author: dylwil3
labels:
  - rule
  - preview
assignees: []
created_at: 2025-06-06T17:07:59Z
updated_at: 2025-06-06T17:10:10Z
url: https://github.com/astral-sh/ruff/issues/18504
synced_at: 2026-01-12T15:54:56Z
```

# [`pyflakes`] Skip `__init__.py` in `undefined-export` (`F822`) if `__getattr__` is overriden at the module level?

---

_@dylwil3_

Would it make any sense to avoid the diagnostic on `__init__.py` files with module-level `__getattr__`? (blocking stabilization for now, I guess)

_Originally posted by @ntBre in https://github.com/astral-sh/ruff/pull/18499#pullrequestreview-2905387270_

For context, see ecosystem results for langchain here: https://github.com/astral-sh/ruff/pull/18499#issuecomment-2949472062

---

_Label `rule` added by @dylwil3 on 2025-06-06 17:08_

---

_Label `preview` added by @dylwil3 on 2025-06-06 17:08_

---

_Renamed from "[`pyflakes`] Skip `__init__.py` if `__getattr__` is overriden at the module level?" to "[`pyflakes`] Skip `__init__.py` in `undefined-export` (`F822`) if `__getattr__` is overriden at the module level?" by @dylwil3 on 2025-06-06 17:10_

---
