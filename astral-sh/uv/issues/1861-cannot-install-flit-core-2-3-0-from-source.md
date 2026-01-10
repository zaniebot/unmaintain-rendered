---
number: 1861
title: "Cannot install `flit-core==2.3.0` from source"
type: issue
state: closed
author: adrianeboyd
labels:
  - compatibility
assignees: []
created_at: 2024-02-22T12:02:40Z
updated_at: 2024-02-25T21:02:59Z
url: https://github.com/astral-sh/uv/issues/1861
synced_at: 2026-01-10T01:23:09Z
---

# Cannot install `flit-core==2.3.0` from source

---

_Issue opened by @adrianeboyd on 2024-02-22 12:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Command, using uv 0.1.7:

```shell
uv pip install "flit-core==2.3.0" --no-cache --no-binary flit-core --reinstall
```

Output:

```none
Resolved 2 packages in 394ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: flit-core==2.3.0
  Caused by: Failed to build: flit-core==2.3.0
  Caused by: Invalid pyproject.toml
  Caused by: TOML parse error at line 4, column 16
  |
4 | backend-path = "."
  |                ^^^
invalid type: string ".", expected a sequence
```

This has been fixed in `flit-core>=3`, but pip installs `flit-core==2.3.0` without errors so pip must be more permissive here. (Actually, it looks the internal type in pip isn't correct in the one place where `backend_path` is typed and then this is unnoticed in practice due to `list(".") == list(["."])`.)

If it weren't a build backend I would not be concerned about an older now-patched version of a library that can't be installed from source, but it's thwarting me in some cases when testing with `--no-binary :all:` for packages with `flit_core<3`.

In general, it might also be useful to have an official supported way to override build requirements, especially given the explicit support of overrides for other purposes?

---

_Label `compatibility` added by @charliermarsh on 2024-02-22 16:42_

---

_Comment by @charliermarsh on 2024-02-25 19:38_

We can make this more permissive.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-25 19:38_

---

_Referenced in [astral-sh/uv#1969](../../astral-sh/uv/pulls/1969.md) on 2024-02-25 20:18_

---

_Closed by @charliermarsh on 2024-02-25 21:02_

---
