```yaml
number: 8266
title: "perf(stdlib): use phf sets for strings"
type: pull_request
state: closed
author: sno2
labels: []
assignees: []
draft: true
base: main
head: perf/phf-sets
created_at: 2023-10-26T23:08:37Z
updated_at: 2023-10-26T23:25:53Z
url: https://github.com/astral-sh/ruff/pull/8266
synced_at: 2026-01-12T02:32:42Z
```

# perf(stdlib): use phf sets for strings

---

_Pull request opened by @sno2 on 2023-10-26 23:08_

## Summary

This might improve performance compared to Rust's default string-matching algorithms.

## Test Plan

This was tested using the existing tests.

---

_Comment by @github-actions[bot] on 2023-10-26 23:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @sno2 on 2023-10-26 23:25_

Seems like Rust's string matching is faster for these cases.

---

_Closed by @sno2 on 2023-10-26 23:25_

---

_Branch deleted on 2023-10-26 23:25_

---
