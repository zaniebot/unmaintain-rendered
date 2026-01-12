```yaml
number: 939
title: "Fix clippy::manual_let_else (pedantic)"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-manual-let-else
created_at: 2022-11-28T06:13:01Z
updated_at: 2023-07-01T20:40:01Z
url: https://github.com/astral-sh/ruff/pull/939
synced_at: 2026-01-12T15:55:05Z
```

# Fix clippy::manual_let_else (pedantic)

---

_@andersk_

This is part of nightly clippy::pedantic for MSRV ≥ 1.65.

---

_Comment by @charliermarsh on 2022-11-28 14:52_

I was a hesitant to change these given that `rustfmt` doesn't yet support them (https://github.com/rust-lang/rustfmt/issues/4914 -- unless that's the wrong issue), but it's a small enough cases that it's probably worth fixing clippy.

---

_Merged by @charliermarsh on 2022-11-28 14:53_

---

_Closed by @charliermarsh on 2022-11-28 14:53_

---

_Branch deleted on 2023-07-01 20:40_

---
