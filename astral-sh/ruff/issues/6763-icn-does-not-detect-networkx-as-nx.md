```yaml
number: 6763
title: "ICN does not detect `networkx as nx`"
type: issue
state: closed
author: AbdealiLoKo
labels:
  - configuration
  - accepted
assignees: []
created_at: 2023-08-22T09:32:05Z
updated_at: 2023-08-22T16:49:05Z
url: https://github.com/astral-sh/ruff/issues/6763
synced_at: 2026-01-12T15:54:46Z
```

# ICN does not detect `networkx as nx`

---

_@AbdealiLoKo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I was using the `flake8-import-conventions`'s `ICN` rules in my project.
I notice that the `networkx as nx` is not implemented in ruff's [settings](https://github.com/astral-sh/ruff/blob/0f9ccfcad9f00e820871fd01e6c3c234cf87ac7b/crates/ruff/src/rules/flake8_import_conventions/settings.rs#L8)

The original library has support for `networkx` in ICN004 - https://github.com/joaopalmeiro/flake8-import-conventions#flake8-codes

TO reproduce:
```
$ cat /tmp/try.py
import networkx

$ ruff /tmp/try.py --isolated --select ICN
$ echo $?
0

$ ruff --version
ruff 0.0.285
```

---

_Comment by @charliermarsh on 2023-08-22 13:38_

This may've just been an oversight. PR welcome if you'd like :)

---

_Label `configuration` added by @charliermarsh on 2023-08-22 13:38_

---

_Label `accepted` added by @charliermarsh on 2023-08-22 13:38_

---

_Comment by @charliermarsh on 2023-08-22 13:46_

This is also configurable: https://beta.ruff.rs/docs/settings/#flake8-import-conventions-aliases.

---

_Closed by @zanieb on 2023-08-22 16:49_

---
