```yaml
number: 186
title: "Fix: Structured output Issue Fix"
type: pull_request
state: merged
author: HallerPatrick
labels: []
assignees: []
merged: true
base: main
head: struct_output
created_at: 2022-09-14T18:17:16Z
updated_at: 2022-09-15T13:43:11Z
url: https://github.com/astral-sh/ruff/pull/186
synced_at: 2026-01-12T05:48:45Z
```

# Fix: Structured output Issue Fix

---

_Pull request opened by @HallerPatrick on 2022-09-14 18:17_

New PR that fixes bug introduced with last PR

---

_Comment by @charliermarsh on 2022-09-14 19:04_

Just for posterity, what was the bug / what's the difference between this PR and the previous?

---

_Comment by @HallerPatrick on 2022-09-15 08:31_

> Just for posterity, what was the bug / what's the difference between this PR and the previous?

Yeah, so running `ruff` once had the expected behaviour, but with the `--watch` flag, nothing was printed to stdout. After some debugging, I discovered that the loop, where the file watcher listens, blocks the printing. This happens with the introduction of the explicit `BufWriter` to stdout. I reverted to using plain `println!` macros, which yields the expected behaviour.  


EDIT: After following my own explanation I figured that removing the `BufWriter` and just use stdout, would also solve the issue....

---

_Merged by @charliermarsh on 2022-09-15 13:43_

---

_Closed by @charliermarsh on 2022-09-15 13:43_

---
