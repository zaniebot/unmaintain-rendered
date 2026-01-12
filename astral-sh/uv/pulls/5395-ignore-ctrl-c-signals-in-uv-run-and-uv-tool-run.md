```yaml
number: 5395
title: "Ignore Ctrl-C signals in `uv run` and `uv tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/sig
created_at: 2024-07-24T04:06:08Z
updated_at: 2024-07-24T13:36:43Z
url: https://github.com/astral-sh/uv/pull/5395
synced_at: 2026-01-12T16:06:47Z
```

# Ignore Ctrl-C signals in `uv run` and `uv tool run`

---

_@charliermarsh_

## Summary

This is a bit simpler than #5333, but seems to work in my testing on macOS and Windows. It's based on implementations that I found in [Pixi](https://github.com/prefix-dev/pixi/blob/36f1bb297db04337172510f41e5b03d7da13c49f/src/cli/exec.rs#L99) and [Wasmer](https://github.com/wasmerio/wasmer/blob/49e60af8df31015cadee063c0e4618b81573fec2/lib/wasix/src/state/builder.rs#L1058).

Closes https://github.com/astral-sh/uv/issues/5257.

## Test Plan

On both macOS and Windows:

- `cargo run -- tool run --from jupyterlab jupyter-lab` -- hit Ctrl-C; verify that the process exits and the terminal is left in a good state.
- `cargo run -- run python` -- hit Ctrl-C; verify that the process does _not_ exit, but does on Ctrl-D.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-24 04:06_

---

_Review requested from @konstin by @charliermarsh on 2024-07-24 04:06_

---

_Label `bug` added by @charliermarsh on 2024-07-24 04:06_

---

_Label `cli` added by @charliermarsh on 2024-07-24 04:06_

---

_Label `preview` added by @charliermarsh on 2024-07-24 04:06_

---

_@konstin approved on 2024-07-24 07:14_

---

_Merged by @charliermarsh on 2024-07-24 12:33_

---

_Closed by @charliermarsh on 2024-07-24 12:33_

---

_Branch deleted on 2024-07-24 12:33_

---

_Comment by @bluss on 2024-07-24 13:36_

Thanks a lot

---
