```yaml
number: 14491
title: "CLI: `ruff rule` should accept human-readable name"
type: issue
state: closed
author: sbrugman
labels: []
assignees: []
created_at: 2024-11-20T15:26:02Z
updated_at: 2024-11-20T15:38:46Z
url: https://github.com/astral-sh/ruff/issues/14491
synced_at: 2026-01-12T15:54:53Z
```

# CLI: `ruff rule` should accept human-readable name

---

_@sbrugman_

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

The `ruff rule` command currently only accepts legacy codes. It should accept the rule name in preparation to the new categorisation:

Command:

```console
ruff rule unused-import
```

Current output:

```text
error: invalid value 'unused-import' for '[RULE]'

For more information, try '--help'.
```

Expected output: the markdown rule documentation for `F401`

---

_Comment by @AlexWaygood on 2024-11-20 15:35_

Thanks! Is this a duplicate of #1773?

---

_Comment by @sbrugman on 2024-11-20 15:37_

It is! I was under the impression that the rule selection already accepted human readable names, but that's not yet the case.

---

_Comment by @AlexWaygood on 2024-11-20 15:38_

Great -- I'll merge this with that issue then :-)

---

_Closed by @AlexWaygood on 2024-11-20 15:38_

---
