```yaml
number: 9081
title: "`uvx --with` does not work with complex version specifiers"
type: issue
state: closed
author: jramcast
labels:
  - bug
assignees: []
created_at: 2024-11-13T08:52:40Z
updated_at: 2024-11-13T16:20:42Z
url: https://github.com/astral-sh/uv/issues/9081
synced_at: 2026-01-12T15:59:41Z
```

# `uvx --with` does not work with complex version specifiers

---

_@jramcast_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The `--with` parameter does not support complex version specifiers:

```console
$ uvx --with "requests>=2.1,<3" pytest
error: Failed to parse: `<3`
  Caused by: Expected package name starting with an alphanumeric character, found `<`
<3
^
```

* uv version:
```console
$ uv --version
uv 0.5.1
```

- Platform: Fedora 41
- Tested on bash and zsh



---

_Label `bug` added by @charliermarsh on 2024-11-13 13:58_

---

_Comment by @charliermarsh on 2024-11-13 13:59_

This is fallout of attempting to support `--with "requests, flask"`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-13 13:59_

---

_Closed by @charliermarsh on 2024-11-13 16:20_

---

_Closed by @charliermarsh on 2024-11-13 16:20_

---
