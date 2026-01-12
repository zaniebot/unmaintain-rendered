```yaml
number: 2915
title: "Rename some `flake8-simplify` rules"
type: pull_request
state: merged
author: ngnpope
labels: []
assignees: []
merged: true
base: main
head: rename-flake8-simplify-rules
created_at: 2023-02-15T10:29:41Z
updated_at: 2023-02-26T23:19:21Z
url: https://github.com/astral-sh/ruff/pull/2915
synced_at: 2026-01-12T04:39:44Z
```

# Rename some `flake8-simplify` rules

---

_Pull request opened by @ngnpope on 2023-02-15 10:29_

Renames the following rules that stood out to me at a glance as needing better names:

- `or-true` to `expr-or-true`
- `and-false` to `expr-and-false`
- `a-or-not-a` to `expr-or-not-expr`
- `a-and-not-a` to `expr-and-not-expr`

Related to #2902.

---

_Comment by @not-my-profile on 2023-02-15 14:14_

I disagree about renaming `needless-bool`, I intentionally named it that in 7e5c19385c02b1f5a3b28b32467d0ea14d3b185e to match the clippy lint [needless_bool](https://rust-lang.github.io/rust-clippy/master/index.html#needless_bool). (Besides `unnecessary-bool-call` doesn't make sense to me  ... what call?).

I agree that the other names you suggest are better but I would rather just merge all of these rules into a single `bool-logic-bug` rule, see also #2714.

---

_Comment by @charliermarsh on 2023-02-26 22:32_

Committing all of these for now apart from `needless-bool`.

---

_Merged by @charliermarsh on 2023-02-26 22:35_

---

_Closed by @charliermarsh on 2023-02-26 22:35_

---

_Branch deleted on 2023-02-26 23:18_

---

_Comment by @ngnpope on 2023-02-26 23:19_

Thanks 👍🏻

---
