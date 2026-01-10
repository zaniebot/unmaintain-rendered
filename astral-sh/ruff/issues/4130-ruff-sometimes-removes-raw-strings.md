```yaml
number: 4130
title: ruff sometimes removes raw strings
type: issue
state: closed
author: m-k-l-s
labels:
  - bug
assignees: []
created_at: 2023-04-27T12:13:29Z
updated_at: 2023-07-14T04:27:47Z
url: https://github.com/astral-sh/ruff/issues/4130
synced_at: 2026-01-10T11:09:47Z
```

# ruff sometimes removes raw strings

---

_Issue opened by @m-k-l-s on 2023-04-27 12:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff (sometimes) turns `r"^.:\\Users\\[^\\]*\\Downloads\\.*"` into `"^.:\\\\Users\\\\[^\\\\]*\\\\Downloads\\\\.*"` which I believe is a bug.

Specifically, this happens (at least) when `not (<something_with_a_raw_string>) is None` is on the input.


```
❯ ruff --version
ruff 0.0.262
```

Using no config, all defaults.

Minimal reasonable reproduction:

```
❯ cat fmt_test.py
import re

# r not removed:
re.search(r"^.:\\Users\\[^\\]*\\Downloads\\.*") is not None
# r removed:
not (re.search(r"^.:\\Users\\[^\\]*\\Downloads\\.*") is None)

```

```
❯ ruff fmt_test.py --diff
--- fmt_test.py
+++ fmt_test.py
@@ -3,4 +3,4 @@
 # r not removed:
 re.search(r"^.:\\Users\\[^\\]*\\Downloads\\.*") is not None
 # r removed:
-not (re.search(r"^.:\\Users\\[^\\]*\\Downloads\\.*") is None)
+re.search("^.:\\\\Users\\\\[^\\\\]*\\\\Downloads\\\\.*") is not None

Would fix 1 error.
```

And this is an ultra minimal reproduction:

`not r"\\" is None` turns into `"\\\\" != None`


---

_Comment by @charliermarsh on 2023-04-27 14:49_

👍 (Internal note: this is a limitation of `Stylist`. Raw strings aren't captured in the AST, so we end up removing them.)

---

_Label `bug` added by @charliermarsh on 2023-04-27 14:49_

---

_Comment by @youknowone on 2023-04-28 08:23_

Is it `Constant`? Will preserving raw string kind including `r` to `Constant` be helpful?

https://github.com/RustPython/RustPython/blob/02840593bc56ae416ba2646166628f1712d6cf43/compiler/parser/src/string.rs#L513

---

_Comment by @addisoncrump on 2023-06-12 16:14_

Of note: the `parser_idempotency` fuzzer finds this bug, but I haven't been reporting these issues since the idempotency didn't seem to be a strong guarantee.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-14 03:04_

---

_Closed by @charliermarsh on 2023-07-14 04:27_

---
