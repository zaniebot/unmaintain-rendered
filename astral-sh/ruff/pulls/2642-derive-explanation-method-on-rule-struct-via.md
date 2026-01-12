```yaml
number: 2642
title: "Derive `explanation` method on Rule struct via rustdoc"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/docs
created_at: 2023-02-07T22:16:46Z
updated_at: 2023-02-07T22:23:30Z
url: https://github.com/astral-sh/ruff/pull/2642
synced_at: 2026-01-12T04:52:00Z
```

# Derive `explanation` method on Rule struct via rustdoc

---

_Pull request opened by @charliermarsh on 2023-02-07 22:16_

```console
❯ cargo run rule B017
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff rule B017`
no-assert-raises-exception

Code: B017 (flake8-bugbear)

### What it does
Checks for `self.assertRaises(Exception)`.

## Why is this bad?
`assertRaises(Exception)` can lead to your test passing even if the
code being tested is never executed due to a typo.

Either assert for a more specific exception (builtin or custom), use
`assertRaisesRegex` or the context manager form of `assertRaises`.
```

---

_Merged by @charliermarsh on 2023-02-07 22:23_

---

_Closed by @charliermarsh on 2023-02-07 22:23_

---

_Branch deleted on 2023-02-07 22:23_

---
