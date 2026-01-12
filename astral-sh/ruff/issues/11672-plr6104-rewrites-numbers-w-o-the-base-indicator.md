```yaml
number: 11672
title: PLR6104 rewrites numbers w/o the base indicator
type: issue
state: closed
author: Avasam
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-06-01T02:13:34Z
updated_at: 2024-12-19T12:58:26Z
url: https://github.com/astral-sh/ruff/issues/11672
synced_at: 2026-01-12T15:54:51Z
```

# PLR6104 rewrites numbers w/o the base indicator

---

_@Avasam_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

input
```py
test = 0x5
test = test + 0xBA

test2 = b""
test2 = test2 + b"\000"
```

Run ` ruff check --fix --unsafe-fixes --preview --select=PLR6104 --isolated`

output
```py
test = 0x5
test += 186

test2 = b""
test2 += b"\x00"
```

Ruff version 0.4.7



---

_Label `fixes` added by @charliermarsh on 2024-06-01 20:18_

---

_Comment by @charliermarsh on 2024-06-01 20:18_

Yeah, common theme with `Generator` usage, we don't preserve verbatim numbers.

---

_Comment by @dhruvmanila on 2024-06-03 10:45_

Once #11457 is completed, this can be solved by including number related flags on `TokenFlags`, propagating it to the `NumberLiteral` node and using that in the `Generator`. This would be similar to the flags on the string nodes. This is actually one of the potential use-case me and Micha discussed for `TokenFlags`.

---

_Closed by @MichaReiser on 2024-12-17 15:07_

---

_Label `bug` added by @dhruvmanila on 2024-12-19 12:58_

---
