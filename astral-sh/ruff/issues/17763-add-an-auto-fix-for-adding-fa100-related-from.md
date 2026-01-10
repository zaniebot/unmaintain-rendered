```yaml
number: 17763
title: "Add an auto fix for adding FA100-related `from __future__ import annotations` imports"
type: issue
state: closed
author: pradyunsg
labels:
  - rule
assignees: []
created_at: 2025-05-01T12:17:45Z
updated_at: 2025-05-05T21:27:14Z
url: https://github.com/astral-sh/ruff/issues/17763
synced_at: 2026-01-10T11:09:58Z
```

# Add an auto fix for adding FA100-related `from __future__ import annotations` imports

---

_Issue opened by @pradyunsg on 2025-05-01 12:17_

### Summary

Currently, it is possible to use https://docs.astral.sh/ruff/rules/missing-required-import/ to add a `from __future__ import annotations` import.

However, this import gets injected into every Python file (which includes a setup.py file, or a plain "contains no code in as a part of a test" file). It's currently not possible to exclude such files from the isort required-import mechanism. It would also be nice to have a `FA200` or similar rule that adds the `__annotations__` import so that a `ruff check --fix --unsafe-fixes --select FA` would modernise the import annotations without needing additional finagling for adding the `__annotations__` import.


---

_Label `fixes` added by @dhruvmanila on 2025-05-01 13:16_

---

_Comment by @MichaReiser on 2025-05-01 17:07_

> However, this import gets injected into every Python file (which includes a setup.py file, or a plain "contains no code in as a part of a test" file).

Isn't it possible to disable the rule for these files using `per-file-ignore`?

---

_Comment by @pradyunsg on 2025-05-01 19:17_

> Isn't it possible to disable the rule for these files using `per-file-ignore`?

It is. I would, however, need to filter the files manually based on whether ruff reports an FA100 on the files (since not every file in the repository contains type annotations that could benefit from the `from __future__ import annotations` import, or contain annotations at all).

---

It's a significantly worse experience than doing something like:

```toml
[lint]
extend-select = ["FA"]
```

and running `ruff check --fix --unsafe-fixes` which appropriately fixes this in the relevant files.

---

_Comment by @pradyunsg on 2025-05-01 19:45_

As a concrete example, I'm trying to modernise pip's annotations and this is non-trivial with FA100 because I have 1752 FA100 errors which are reported -- fixing which would involve me either (a) adding `from __future__ import annotations` to ~250 files by hand-curating that list, or (b) exclude ~100 files from the isort required-imports check.


---

_Comment by @MichaReiser on 2025-05-02 07:25_

Thanks. That makes sense. What I understand is that it could make sense for this to be its own rule that is cleverer than the isort rule

---

_Label `fixes` removed by @MichaReiser on 2025-05-02 07:25_

---

_Label `rule` added by @MichaReiser on 2025-05-02 07:25_

---

_Comment by @eli-schwartz on 2025-05-04 19:33_

This was previously requested at https://github.com/astral-sh/ruff/issues/13273

Also, https://github.com/snok/flake8-type-checking/issues/190 (implemented but doesn't do autofixes)

---

_Closed by @MichaReiser on 2025-05-05 05:54_

---

_Comment by @pradyunsg on 2025-05-05 21:27_

Whoops, that didn't show up in my searches. Thanks @eli-schwartz!

---
