---
number: 10034
title: "[Infinite loop] by the rule codes E302"
type: issue
state: closed
author: Fokko
labels:
  - needs-info
assignees: []
created_at: 2024-02-19T01:38:39Z
updated_at: 2024-03-29T20:03:51Z
url: https://github.com/astral-sh/ruff/issues/10034
synced_at: 2026-01-10T01:22:49Z
---

# [Infinite loop] by the rule codes E302

---

_Issue opened by @Fokko on 2024-02-19 01:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```
...quoting the contents of `pyiceberg/avro/decoder_fast.pyi`, the rule codes E302, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```

This can be easily be reproduced by:

```
git clone git@github.com:apache/iceberg-python.git
cd iceberg-python
pre-commit autoupdate
make lint
```

Ruff version v0.2.2



---

_Referenced in [apache/iceberg-python#442](../../apache/iceberg-python/pulls/442.md) on 2024-02-19 01:41_

---

_Comment by @MichaReiser on 2024-02-19 07:50_

Hy @Fokko . Thanks for reporting this issue. Any change that you've already updated your repository? I'm asking because I'm unable to reproduce the issue after cloning your repository and running 

```
 ruff check --select E302 --preview --fix
```

---

_Label `needs-info` added by @MichaReiser on 2024-02-19 07:50_

---

_Closed by @charliermarsh on 2024-03-29 20:03_

---

_Comment by @charliermarsh on 2024-03-29 20:03_

Closing, but happy to re-open if we have a repro.

---
