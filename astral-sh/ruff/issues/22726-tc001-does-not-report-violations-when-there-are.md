```yaml
number: 22726
title: TC001 does not report violations when there are other modules from the same package not violating
type: issue
state: open
author: samueljsb
labels:
  - bug
assignees: []
created_at: 2026-01-19T15:59:06Z
updated_at: 2026-01-19T21:17:40Z
url: https://github.com/astral-sh/ruff/issues/22726
synced_at: 2026-01-19T21:32:54Z
```

# TC001 does not report violations when there are other modules from the same package not violating

---

_@samueljsb_

### Summary

This is best described with an example. Consider a module (`t.py`) that imports both `my_pkg.a` and `my_pkg.b` and uses one in an annotation and another in runtime code:

```python
from __future__ import annotations

from my_pkg import a
from my_pkg import b

def f(obj: a.A | b.B) -> bool:
    b.B()
```

This will not report any errors:

```console
$ ruff check --select=TC t.py
All checks passed!
```

However, if we change the module so it no longer uses `my_pkg.b` at runtime:

```diff
  from __future__ import annotations

  from my_pkg import a
  from my_pkg import b

  def f(obj: a.A | b.B) -> bool:
-     b.B()
+     pass
```

We now get an error for *both* imports:

```console
$ ruff check --select=TC t.py
TC001 Move application import `my_pkg.a` into a type-checking block
 --> t.py:3:20
  |
1 | from __future__ import annotations
2 |
3 | from my_pkg import a
  |                    ^
4 | from my_pkg import b
  |
help: Move into type-checking block

TC001 Move application import `my_pkg.b` into a type-checking block
 --> t.py:4:20
  |
3 | from my_pkg import a
4 | from my_pkg import b
  |                    ^
5 |
6 | def f(obj: a.A | b.B) -> bool:
  |
help: Move into type-checking block

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

I would expect the import of `my_pkg.a` to have caused an error for both examples, with `my_pkg.b` also becoming an error when it was no longer used.

### Version

ruff 0.14.13 (b4b8299d6 2026-01-15)

---

_Renamed from "TC001 does not report violations when there are other modules from the same package no violating" to "TC001 does not report violations when there are other modules from the same package not violating" by @samueljsb on 2026-01-19 15:59_

---

_Comment by @ntBre on 2026-01-19 16:15_

Thanks for the report! I think this may be a duplicate of #15723, but I'm not totally sure. The underlying cause sounds similar, but the specific example is a bit different. @Daverball may know better than me.

---

_Label `bug` added by @ntBre on 2026-01-19 16:15_

---

_Comment by @Daverball on 2026-01-19 21:17_

This is working as designed. This behavior can be changed by setting [`lint.flake8-type-checking.strict`](https://docs.astral.sh/ruff/settings/#lint_flake8-type-checking_strict) to `true`.

The reason it doesn't report these by default, is because there's usually no performance benefit. Since for non-module imports (which are the common case for `from` imports) the module is only imported once either way, so emitting an error for this would be causing unnecessary churn. But since some people prefer consistency, there is a setting that allows you to override this decision.

---
