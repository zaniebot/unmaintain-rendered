```yaml
number: 1901
title: "Lock `stdout` once when printing diagnostics"
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: printer-lock-stdout
created_at: 2023-01-16T01:58:49Z
updated_at: 2023-01-16T02:04:55Z
url: https://github.com/astral-sh/ruff/pull/1901
synced_at: 2026-01-12T15:55:07Z
```

# Lock `stdout` once when printing diagnostics

---

_@messense_

https://doc.rust-lang.org/stable/std/io/struct.Stdout.html

> Each handle shares a global buffer of data to be written to the standard output stream.
> Access is also synchronized via a lock and
> explicit control over locking is available via the [`lock`](https://doc.rust-lang.org/stable/std/io/struct.Stdout.html#method.lock) method.

See also

* [Why is stdout behind a lock in the first place?](https://www.reddit.com/r/rust/comments/aen8t4/why_is_stdout_behind_a_lock_in_the_first_place/)
* [Raw stdout write performance go vs rust](https://www.reddit.com/r/rust/comments/qiziym/raw_stdout_write_performance_go_vs_rust/)

---

_Renamed from "Lock `stdout` when printing diagnostics" to "Lock `stdout` once when printing diagnostics" by @messense on 2023-01-16 01:59_

---

_Merged by @charliermarsh on 2023-01-16 02:04_

---

_Closed by @charliermarsh on 2023-01-16 02:04_

---

_Branch deleted on 2023-01-16 02:04_

---
