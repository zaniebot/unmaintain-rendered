```yaml
number: 2002
title: Fix that --explain panics
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: fix-explain-panic
created_at: 2023-01-19T16:59:18Z
updated_at: 2023-01-19T17:58:55Z
url: https://github.com/astral-sh/ruff/pull/2002
synced_at: 2026-01-12T15:55:07Z
```

# Fix that --explain panics

---

_@not-my-profile_

This commit fixes a bug accidentally introduced in 6cf770a6924e6c2b00f53a52492bdffe80015552,
which resulted every `ruff --explain <code>` invocation to fail with:

    thread 'main' panicked at 'Mismatch between definition and access of `explain`.
    Could not downcast to ruff::registry::Rule, need to downcast to &ruff::registry::Rule',
    ruff_cli/src/cli.rs:184:18

We also add an integration test for --explain to prevent such bugs from going by unnoticed in the future.

---

_Merged by @charliermarsh on 2023-01-19 17:58_

---

_Closed by @charliermarsh on 2023-01-19 17:58_

---

_Comment by @charliermarsh on 2023-01-19 17:58_

Thank you for following up, and for adding a test here.

---
