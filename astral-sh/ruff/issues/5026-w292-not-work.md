```yaml
number: 5026
title: "`W292` not work"
type: issue
state: closed
author: Pagliacii
labels:
  - question
assignees: []
created_at: 2023-06-12T15:40:29Z
updated_at: 2023-06-12T16:07:52Z
url: https://github.com/astral-sh/ruff/issues/5026
synced_at: 2026-01-10T11:09:47Z
```

# `W292` not work

---

_Issue opened by @Pagliacii on 2023-06-12 15:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
ruff 0.0.272

```shell
λ ruff check hello.py --select W292 -v --fix --isolated
[2023-06-12][23:37:50][ruff_cli::commands::run][DEBUG] Identified files to lint in: 8.23ms
[2023-06-12][23:37:50][ruff_cli::diagnostics][DEBUG] Checking: R:\code\python\hello\hello.py
[2023-06-12][23:37:50][ruff_cli::commands::run][DEBUG] Checked 1 files in: 952µs

λ cat hello.py
#!/usr/bin/env python3
# -*- coding=utf-8 -*-

print("Hello")
```

It doesn't end with a new line.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-12 15:43_

---

_Comment by @charliermarsh on 2023-06-12 15:43_

Will take a look.

---

_Comment by @charliermarsh on 2023-06-12 15:45_

I believe this is working as intended. Can you upload the exact file after autofixing?

---

_Label `question` added by @charliermarsh on 2023-06-12 15:45_

---

_Comment by @charliermarsh on 2023-06-12 15:47_

Or `cat -e hello.py`, which will show you whether there's a trailing newline on the last line.

---

_Comment by @charliermarsh on 2023-06-12 16:00_

Closing for now but can follow-up if there's a clear repro!

---

_Closed by @charliermarsh on 2023-06-12 16:00_

---

_Comment by @Pagliacii on 2023-06-12 16:07_

```shell
#!/usr/bin/env python3^M$
# -*- coding=utf-8 -*-^M$
^M$
print("Hello World")^M$
```

Seems I misunderstood the rule. Thx

---
