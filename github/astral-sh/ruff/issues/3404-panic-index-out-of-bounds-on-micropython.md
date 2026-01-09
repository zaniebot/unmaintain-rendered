---
number: 3404
title: "[Panic] index out of bounds on micropython/micropython"
type: issue
state: closed
author: cclauss
labels:
  - bug
assignees: []
created_at: 2023-03-08T14:45:09Z
updated_at: 2023-03-13T04:02:59Z
url: https://github.com/astral-sh/ruff/issues/3404
synced_at: 2026-01-07T13:12:14-06:00
---

# [Panic] index out of bounds on micropython/micropython

---

_Issue opened by @cclauss on 2023-03-08 14:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
ruff --version  # --> ruff 0.0.254
git clone https://github.com/micropython/micropython
cd micropython
RUST_BACKTRACE=1 ruff .
```
% `RUST_BACKTRACE=1 ruff --exclude=tests . ` # works as expected.
```
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'index out of bounds: the len is 1 but the index is 1', crates/ruff/src/checkers/noqa.rs:42:38
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
[1]    38956 abort      RUST_BACKTRACE=1 ruff .
```


---

_Renamed from "[Panic]" to "[Panic] index out of bounds on micropython/micropython" by @cclauss on 2023-03-08 14:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-08 15:58_

---

_Label `bug` added by @charliermarsh on 2023-03-08 15:58_

---

_Comment by @charliermarsh on 2023-03-08 15:58_

Thanks! Will take a look today.

---

_Comment by @charliermarsh on 2023-03-08 19:02_

It's panicking on this test file which contains CR line endings: `micropython/tests/basics/string_cr_conversion.py`.

(\cc @MichaReiser for visibility, think you had a comment somewhere around "continuation on panic".)

---

_Comment by @charliermarsh on 2023-03-08 19:05_

As good a time as any to implement graceful fallback for CR line endings.

---

_Referenced in [astral-sh/ruff#3454](../../astral-sh/ruff/pulls/3454.md) on 2023-03-12 04:29_

---

_Comment by @charliermarsh on 2023-03-13 04:02_

Resolved via #3454 and #3439.

---

_Closed by @charliermarsh on 2023-03-13 04:02_

---
