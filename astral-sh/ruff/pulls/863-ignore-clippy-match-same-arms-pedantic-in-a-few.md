```yaml
number: 863
title: "Ignore clippy::match-same-arms (pedantic) in a few places"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-match-same-arms
created_at: 2022-11-21T19:23:33Z
updated_at: 2022-11-21T20:59:59Z
url: https://github.com/astral-sh/ruff/pull/863
synced_at: 2026-01-12T15:55:05Z
```

# Ignore clippy::match-same-arms (pedantic) in a few places

---

_@andersk_

“this match arm has an identical body to another arm”

https://rust-lang.github.io/rust-clippy/master/index.html#match_same_arms

We probably wouldn’t enable this lint, but ignoring it in these three functions makes `cargo clippy -- -W clippy::pedantic` run much faster.

---

_Merged by @charliermarsh on 2022-11-21 20:59_

---

_Closed by @charliermarsh on 2022-11-21 20:59_

---
