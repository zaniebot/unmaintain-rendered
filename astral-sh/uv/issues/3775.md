```yaml
number: 3775
title: "`uv tool run` failed with `No such file or directory` error when there is no cache directory"
type: issue
state: closed
author: njzjz
labels:
  - bug
assignees: []
created_at: 2024-05-22T22:20:12Z
updated_at: 2024-05-22T22:59:37Z
url: https://github.com/astral-sh/uv/issues/3775
synced_at: 2026-01-10T05:31:37Z
```

# `uv tool run` failed with `No such file or directory` error when there is no cache directory

---

_Issue opened by @njzjz on 2024-05-22 22:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv 0.2.2, on Linux

When there is no cache directory (which is common in a fresh CI environment), `uv tool run` will fail with `No such file or directory` error.

Reproduction:

```sh
$ mv $HOME/.cache/uv $HOME/.cache/uv_bak
$ uv tool run  --verbose uv
warning: `uv tool run` is experimental and may change without warning.
DEBUG Syncing ephemeral environment.
DEBUG Searching for interpreter that fulfills Python @ default
error: No such file or directory (os error 2) at path "/home/jz748/.cache/uv/.tmpI48ZiF"
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 22:49_

---

_Label `bug` added by @charliermarsh on 2024-05-22 22:49_

---

_Comment by @charliermarsh on 2024-05-22 22:50_

Thanks, sorry, this was the result of a clean merge that required changes.

---

_Closed by @charliermarsh on 2024-05-22 22:59_

---
