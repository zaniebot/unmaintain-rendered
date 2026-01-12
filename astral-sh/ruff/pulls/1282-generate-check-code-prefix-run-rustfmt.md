```yaml
number: 1282
title: "generate-check-code-prefix: Run `rustfmt` automatically; only write if changed"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: generate-check-code-prefix
created_at: 2022-12-18T20:12:18Z
updated_at: 2022-12-18T22:46:23Z
url: https://github.com/astral-sh/ruff/pull/1282
synced_at: 2026-01-12T05:36:31Z
```

# generate-check-code-prefix: Run `rustfmt` automatically; only write if changed

---

_Pull request opened by @andersk on 2022-12-18 20:12_

This avoids unnecessary recompilation when nothing changed.

---

_@squiddy reviewed on 2022-12-18 21:01_

---

_Review comment by @squiddy on `ruff_dev/Cargo.toml`:19 on 2022-12-18 21:01_

Just curious, why the dependency on this crate and not `std::process`?

---

_@andersk reviewed on 2022-12-18 21:42_

---

_Review comment by @andersk on `ruff_dev/Cargo.toml`:19 on 2022-12-18 21:42_

`std::process` doesn’t have the functionality of simultaneously writing to and reading from the child in a [deadlock-free way](https://docs.rs/subprocess/latest/subprocess/struct.Popen.html#method.communicate_start):

> The difference between this and simply writing input data to `self.stdin` and then reading output from `self.stdout` and `self.stderr` is that the reading and the writing are performed simultaneously. A naive implementation that writes and then reads has an issue when the subprocess responds to part of the input by providing output. The output must be read for the subprocess to accept further input, but the parent process is still blocked on writing the rest of the input daata. Since neither process can proceed, a deadlock occurs. This is why a correct implementation must write and read at the same time.

But actually, it looks like `rustfmt` always buffers all input before starting to write, so maybe `std::process` is fine.

---

_Merged by @charliermarsh on 2022-12-18 22:46_

---

_Closed by @charliermarsh on 2022-12-18 22:46_

---
