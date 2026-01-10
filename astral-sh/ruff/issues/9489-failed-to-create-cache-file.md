```yaml
number: 9489
title: Failed to create cache file
type: issue
state: closed
author: yellowhat
labels: []
assignees: []
created_at: 2024-01-12T09:26:15Z
updated_at: 2024-01-12T09:34:05Z
url: https://github.com/astral-sh/ruff/issues/9489
synced_at: 2026-01-10T11:09:51Z
```

# Failed to create cache file

---

_Issue opened by @yellowhat on 2024-01-12 09:26_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


Hi,
with the latest release (0.1.12), I get the following error:

```console
$ pip install ruff==0.1.12
$ ruff check --diff .
ruff failed
  Cause: Failed to create cache file '/data/.ruff_cache/0.1.12/1566177860923460140'
  Cause: No such file or directory (os error 2)
```

It seems that now it expect the cache folder to already exist.

The workaround is to run `mkdir -p .ruff_cache/0.1.12`, that is quite annoying.

---

_Comment by @MichaReiser on 2024-01-12 09:34_

Sorry for the trouble. This has been fixed very recently and we plan to make a bugfix release soon. See https://github.com/astral-sh/ruff/issues/9478

---

_Closed by @MichaReiser on 2024-01-12 09:34_

---
