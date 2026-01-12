```yaml
number: 10849
title: "inconsistency between `uv sync` and `uv pip install`"
type: issue
state: closed
author: Diddy42
labels:
  - bug
assignees: []
created_at: 2025-01-22T10:21:40Z
updated_at: 2025-01-22T12:08:05Z
url: https://github.com/astral-sh/uv/issues/10849
synced_at: 2026-01-12T16:00:22Z
```

# inconsistency between `uv sync` and `uv pip install`

---

_@Diddy42_

### Summary

There seem to be cases in which `uv sync` is not able to resolve a project's environment while `uv pip install` does.

The following example was executed inside the `ghcr.io/astral-sh/uv:python3.12-bookworm` image.

With this pyproject.toml:
```toml
[project]
name = "teeeeest"
version = "0.1.0"
requires-python = ">=3.8,<3.9"
classifiers = []
dependencies = [
  "tensorflow==2.12.0",
]
```

`uv sync` fails to resolve because it decides to use `tensorflow-io-gcs-filesystem==0.35.0`, which has no compatible platforms.

But explicitly creating a pyhton 3.8 environment and then installing the dependency works:

`uv venv --python 3.8 && uv pip install tensorflow==2.12.0`

This time resolving to use `tensorflow-io-gcs-filesystem==0.34.0`.

Additionally, if now I try to help `uv` by explicitly listing `tensorflow-io-gcs-filesystem==0.34.0` in the pyproject's dependencies, `uv sync` works without issues.

This seems like a bug to me, but in case I'm just misunderstanding the purpose of `uv sync` feel free to close this

### Platform

Linux 5.15.153.1-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

0.5.22

### Python version

3.8.20

---

_Label `bug` added by @Diddy42 on 2025-01-22 10:21_

---

_Label `bug` removed by @konstin on 2025-01-22 12:02_

---

_Label `question` added by @konstin on 2025-01-22 12:02_

---

_Label `question` removed by @konstin on 2025-01-22 12:06_

---

_Label `bug` added by @konstin on 2025-01-22 12:06_

---

_Comment by @konstin on 2025-01-22 12:08_

This is a known bug and another case that would be solved by https://github.com/astral-sh/uv/pull/10067, we're tracking all those cases in #9711.

---

_Closed by @konstin on 2025-01-22 12:08_

---
