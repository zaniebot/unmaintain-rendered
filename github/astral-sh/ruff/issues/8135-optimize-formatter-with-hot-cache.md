---
number: 8135
title: Optimize formatter with hot cache
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-10-23T10:06:06Z
updated_at: 2024-05-04T21:22:53Z
url: https://github.com/astral-sh/ruff/issues/8135
synced_at: 2026-01-07T13:12:15-06:00
---

# Optimize formatter with hot cache

---

_Issue opened by @konstin on 2023-10-23 10:06_

There's a lot of optimization opportunities with the formatter with a hot cache (the example is apache airflow with rayon removed):

* Sorting the result takes a long time so does building the result vec, even though we know its approximate size
* #8132
* Deserializing the cache takes noticeable time even if we just need 4k paths plus timestamps
* We first iterate over each directory only to check the timestamps later, could those be combined?

![image](https://github.com/astral-sh/ruff/assets/6826232/bc47843b-64c3-467c-ac76-99d1df836cb0)

Instructions for generating the flamegraph:
 * Install perf and `cargo install flamegraph`
 * git clone apache airflow
 * Apply https://gist.github.com/konstin/2e8c45c7f1fa53dc003833870226bab8
 * `flamegraph -F 9999 -- target/release-debug/ruff format ../airflow/` (Set your cpu to powersave for more samples)


---

_Label `formatter` added by @konstin on 2023-10-23 10:06_

---

_Comment by @charliermarsh on 2023-10-24 19:06_

Fixing the sort here.

---

_Referenced in [astral-sh/ruff#8181](../../astral-sh/ruff/pulls/8181.md) on 2023-10-24 20:09_

---

_Comment by @charliermarsh on 2024-05-04 21:22_

We did a bunch of stuff here.

---

_Closed by @charliermarsh on 2024-05-04 21:22_

---
