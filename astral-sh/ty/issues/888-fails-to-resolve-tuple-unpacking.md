```yaml
number: 888
title: fails to resolve tuple unpacking
type: issue
state: closed
author: jc-louis
labels: []
assignees: []
created_at: 2025-07-25T09:17:04Z
updated_at: 2025-07-25T14:18:29Z
url: https://github.com/astral-sh/ty/issues/888
synced_at: 2026-01-10T02:06:24Z
```

# fails to resolve tuple unpacking

---

_Issue opened by @jc-louis on 2025-07-25 09:17_

### Summary

Hello,
I get this error locally with ty `0.0.1-alpha.15` (`uvx ty check file.py`)
```py
start_end = (0, 10)
range(*start_end)

# error[no-matching-overload] file.py:2:12: No overload of function `__new__` matches arguments
```
but cannot reproduce in playground

### Version

0.0.1-alpha.15

---

_Comment by @sharkdp on 2025-07-25 09:38_

Thank you for reporting this

> but cannot reproduce in playground

Yes, this was recently fixed in https://github.com/astral-sh/ruff/pull/18996. The playground also runs the latest version of `ty` from the `main` branch. It should start to work in the CLI in the next release.

---

_Closed by @sharkdp on 2025-07-25 09:38_

---

_Comment by @sharkdp on 2025-07-25 14:18_

Fixed in [ty 0.0.1-alpha.16](https://github.com/astral-sh/ty/releases/tag/0.0.1-alpha.16)

---
