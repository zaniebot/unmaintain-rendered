```yaml
number: 5225
title: "[Panic] in pre-commit with v0.0.273"
type: issue
state: closed
author: stumpylog
labels:
  - bug
assignees: []
created_at: 2023-06-20T22:06:20Z
updated_at: 2023-06-20T23:01:22Z
url: https://github.com/astral-sh/ruff/issues/5225
synced_at: 2026-01-12T15:54:45Z
```

# [Panic] in pre-commit with v0.0.273

---

_@stumpylog_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

After updating my pre-commit hook from 0.0.272 to 0.0.273, running `ruff` via pre-commit fails with a panic.  Running ruff manually at the same version works without issue.  There's no pyproject, and my .ruff.toml was unchanged. Running just the ruff hook also works find. I've removed `~/.cache/pre-commit` to get a fresh copy, which still fails.

After setting RUST_BACKTRACE=1, the log is as below:

```log
ruff.....................................................................Failed
- hook id: ruff
- exit code: 101

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:
error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:112:65
112:65
stack backtrace:

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:112:65

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:112:65

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:112:65

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread '<unnamed>' panicked at 'called `Option::unwrap()` on a `None` value', crates\ruff_cli\src\commands\run.rs:112:65
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```


---

_Renamed from "[Panic]" to "[Panic] in pre-commit with v0.0.273" by @stumpylog on 2023-06-20 22:06_

---

_Comment by @hauntsaninja on 2023-06-20 22:26_

I'm running into this too, cc @Thomasdezeeuw since the panicking line blames to https://github.com/astral-sh/ruff/pull/5120

---

_Comment by @charliermarsh on 2023-06-20 22:31_

I don’t know the cause, but I’ll cut a fix for this tonight.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-20 22:33_

---

_Label `bug` added by @charliermarsh on 2023-06-20 22:33_

---

_Comment by @charliermarsh on 2023-06-20 22:37_

I'm taking a look now.

---

_Comment by @charliermarsh on 2023-06-20 22:42_

Are any of these projects public, to help me reproduce?

---

_Comment by @hauntsaninja on 2023-06-20 22:46_

I don't have a public reproducer. If you aren't able to reproduce anywhere, I can try to make one.

---

_Closed by @charliermarsh on 2023-06-20 23:01_

---
