---
number: 3507
title: Fuzzer support
type: issue
state: closed
author: qarmin
labels:
  - internal
assignees: []
created_at: 2023-03-14T10:48:49Z
updated_at: 2023-06-08T16:03:05Z
url: https://github.com/astral-sh/ruff/issues/3507
synced_at: 2026-01-10T01:22:42Z
---

# Fuzzer support

---

_Issue opened by @qarmin on 2023-03-14 10:48_

Recently I tried to create simple automatic fuzzer to test project to find crashes/leaks but I got few problems.

Current patch (feel free to take it, fix/modify and create PR) - [0002-Ruff-fuzzer.patch.txt](https://github.com/charliermarsh/ruff/files/10967369/0002-Ruff-fuzzer.patch.txt)


- `lint_stdin` is not available from outside of module (`pub mod diagnostics;` fixes it)
- Cannot easy create settings with all available rules enabled - for testing purposes I enabled `for_rules` constructor for non test builds and I set there random one rule
- Looks that `lint_stdin` finds different problems that default ruff command(probably `lint_path`).
When testing, after a while(fuzzer need to start to test longer inputs) I see in terminal messages
```
error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `-`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```
but when I check file with `ruff 256274.py --fix`([256274.py.zip](https://github.com/charliermarsh/ruff/files/10967634/256274.py.zip)) command, then I got parse error message
```
error: Failed to parse pp/256274.py: invalid syntax. Got unexpected token ';' at line 1 column 7
```

I already used fuzzer to minimize test case from https://github.com/charliermarsh/ruff/issues/3425, so I can confirm that it works(at least partially)

After applying patch, to run fuzzer you need to run this commands
```
cargo install cargo-fuzz
cd ruff
cargo +nightly fuzz run fuzz_target_1
```

---

_Referenced in [astral-sh/ruff#3524](../../astral-sh/ruff/issues/3524.md) on 2023-03-14 21:18_

---

_Label `internal` added by @charliermarsh on 2023-03-15 02:24_

---

_Referenced in [astral-sh/ruff#3721](../../astral-sh/ruff/issues/3721.md) on 2023-03-26 14:33_

---

_Comment by @charliermarsh on 2023-06-08 16:03_

We added fuzzer support recently (#4822).

---

_Closed by @charliermarsh on 2023-06-08 16:03_

---
