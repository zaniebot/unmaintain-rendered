```yaml
number: 497
title: Fix “not a char boundary” error with Unicode in extract_quote
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: extract_quote
created_at: 2022-10-28T22:51:11Z
updated_at: 2022-11-20T01:59:34Z
url: https://github.com/astral-sh/ruff/pull/497
synced_at: 2026-01-12T15:55:04Z
```

# Fix “not a char boundary” error with Unicode in extract_quote

---

_@andersk_

Fixes this error:

```console
$ echo 's = "Δx"' | ruff --select=W605 -
thread 'main' panicked at 'byte index 2 is not a char boundary; it is inside 'Δ' (bytes 1..3) of `"Δx"`', src/pycodestyle/checks.rs:252:23
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Merged by @charliermarsh on 2022-10-28 22:59_

---

_Closed by @charliermarsh on 2022-10-28 22:59_

---

_Branch deleted on 2022-11-20 01:59_

---
