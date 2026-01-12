```yaml
number: 95
title: "perf: Avoid `Vec<&str>` allocation during line length checking"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/no-alloc-line-check
created_at: 2022-09-03T19:02:58Z
updated_at: 2022-09-03T20:34:11Z
url: https://github.com/astral-sh/ruff/pull/95
synced_at: 2026-01-12T15:55:04Z
```

# perf: Avoid `Vec<&str>` allocation during line length checking

---

_@Stranger6667_

I noticed that for each line a new vector is allocated, however it could be avoided. Generally, this allocation doesn't hurt much, but it could be visible on lines with a lot of short words and is invoked on **every** line `ruff` processes. For example, the [\_\_init\_\_.py](https://github.com/psf/black/blob/main/src/black/__init__.py) (1388 lines) file from `black` gains ~35% improvement:
- before: `236.92 µs`
- after: `152.35 µs`

Here is the data for the example used to test E501 (~45% improvement):

- before: `781.69 ns`
- after: `430.58 ns`

NOTE: I measured only `check_lines` performance with a single check enabled.

P.S. Sorry if I am nitpicking too much about performance (as it is likely not the most impactful thing at this stage).

---

_@charliermarsh approved on 2022-09-03 20:26_

This is great! Thank you!

---

_Merged by @charliermarsh on 2022-09-03 20:27_

---

_Closed by @charliermarsh on 2022-09-03 20:27_

---

_Branch deleted on 2022-09-03 20:34_

---
