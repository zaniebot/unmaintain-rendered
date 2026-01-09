---
number: 13871
title: "`# noqa` doesn't stop import sorting (I001: `unsorted-imports`)"
type: issue
state: closed
author: samuelcolvin
labels:
  - question
assignees: []
created_at: 2024-10-22T03:23:12Z
updated_at: 2025-03-18T07:05:14Z
url: https://github.com/astral-sh/ruff/issues/13871
synced_at: 2026-01-07T13:12:16-06:00
---

# `# noqa` doesn't stop import sorting (I001: `unsorted-imports`)

---

_Issue opened by @samuelcolvin on 2024-10-22 03:23_

I want to add the following after other imports

```py
# if you don't want to use logfire, just comment out these lines
import logfire
logfire.configure()
```

(I want to keep the two lines of code together so they make sense, and are easy to comment out for users)

Neither `# noqa` on any of the lines, nor `# fmt: off` stops ruff (`uv run ruff check --fix --fix-only`) from rewriting the imports and moving the import away from `logfire.configure()`.

Am I missing something?


---

_Comment by @samuelcolvin on 2024-10-22 03:35_

My current workaround is to add

```toml
"examples/**.py" = ["I001"]
```

to `pyproject.toml` to disable import sorting in all examples which is a bit unfortunate.

---

_Renamed from "`# noqa` doesn't stop import rewriting" to "`# noqa` doesn't stop import sorting (I001: `unsorted-imports`)" by @samuelcolvin on 2024-10-22 03:36_

---

_Comment by @MichaReiser on 2024-10-22 06:35_

I can see how this is surprising. We should reduce the number of pragma comments 

Import sorting has its own `isort: skip` pragma comments to disable sorting, see [action comments](https://docs.astral.sh/ruff/linter/#action-comments)

What should work in your case is to do:

```python
# if you don't want to use logfire, just comment out these lines
import logfire  # isort: skip
logfire.configure()
```

---

_Comment by @dhruvmanila on 2024-10-22 08:04_

`# noqa` seems to be working fine: https://play.ruff.rs/534e9adb-7fb7-40bf-892c-191bff109019

---

_Label `question` added by @MichaReiser on 2025-03-18 07:05_

---

_Closed by @MichaReiser on 2025-03-18 07:05_

---
