```yaml
number: 8136
title: "The uv.lock doesn't record wheels when the requires-python is an exact version"
type: issue
state: closed
author: frostming
labels:
  - bug
assignees: []
created_at: 2024-10-12T00:28:20Z
updated_at: 2024-10-12T07:23:05Z
url: https://github.com/astral-sh/uv/issues/8136
synced_at: 2026-01-12T15:59:20Z
```

# The uv.lock doesn't record wheels when the requires-python is an exact version

---

_@frostming_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Given this pyproject.toml:

```toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12"
dependencies = [
    "lxml>=5.3.0",
]
```

## Actual result
Run `uv lock`, the result `uv.lock` is:

```toml
version = 1
requires-python = "==3.12"

[[package]]
name = "lxml"
version = "5.3.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/e7/6b/20c3a4b24751377aaa6307eb230b66701024012c29dd374999cc92983269/lxml-5.3.0.tar.gz", hash = "sha256:4e109ca30d1edec1ac60cdbe341905dc3b8f55b16855e03a54aaf59e51ec8c6f", size = 3679318 }

[[package]]
name = "test-uv"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "lxml" },
]

[package.metadata]
requires-dist = [{ name = "lxml", specifier = ">=5.3.0" }]
```
The wheels are missing.

## Expected result

The wheels for `cp312-cp312` should be recorded.

---

_Comment by @charliermarsh on 2024-10-12 03:49_

That is... interesting. Thanks!

---

_Label `bug` added by @charliermarsh on 2024-10-12 03:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-12 04:07_

---

_Closed by @charliermarsh on 2024-10-12 04:17_

---

_Comment by @frostming on 2024-10-12 07:22_

Wow that was fast 🎉 

---
