```yaml
number: 621
title: Improve discoverability of dev commands
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: ergonomic-ruff-dev
created_at: 2022-11-06T12:55:01Z
updated_at: 2022-12-27T05:42:50Z
url: https://github.com/astral-sh/ruff/pull/621
synced_at: 2026-01-12T15:55:05Z
```

# Improve discoverability of dev commands

---

_@squiddy_

Just some smaller changes of things I noticed during/after my first contribution. I completely missed the print-ast command and it took me a bit to figure out how to properly call it (although that is just my lack of rust/cargo experience likely).

I saw this neat cargo alias in two other rust projects and I think this makes it nicer to use, e.g. `cargo dev generate-rules-table`. Also generate-rules-table was really expecting to be called from within the ruff_dev directory, so I made that more flexible.

---

_Comment by @charliermarsh on 2022-11-06 19:25_

Yeah this is great! Thank you.

---

_Comment by @charliermarsh on 2022-11-06 19:25_

I'm going to make some touch-ups to `CONTRIBUTING.md` separately.

---

_Merged by @charliermarsh on 2022-11-06 19:25_

---

_Closed by @charliermarsh on 2022-11-06 19:25_

---

_Branch deleted on 2022-12-27 05:42_

---
