```yaml
number: 2145
title: "refactor CLI (1/3): Cosmetic changes"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: rename-Cli-to-Args
created_at: 2023-01-25T03:06:02Z
updated_at: 2023-01-26T05:55:51Z
url: https://github.com/astral-sh/ruff/pull/2145
synced_at: 2026-01-12T15:55:07Z
```

# refactor CLI (1/3): Cosmetic changes

---

_@not-my-profile_

While working on the many-to-many mapping I wanted to introduce a new subcommand ... only to end up refactoring the whole `ruff_cli::cli::Cli` struct.

This CLI refactor has gotten so large that I'll split it up in two PRs (this PR contains the purely cosmetic changes, the next PR will contain the larger refactors).

---

_Renamed from "refactor: Rename CLI arg structs from Cli to Args" to "refactor CLI (1/2): Cosmetic refactors" by @not-my-profile on 2023-01-25 04:27_

---

_Renamed from "refactor CLI (1/2): Cosmetic refactors" to "refactor CLI (1/2): Cosmetic changes" by @not-my-profile on 2023-01-25 04:28_

---

_Comment by @charliermarsh on 2023-01-26 00:24_

LGTM, but now has a conflict (sorry!)

---

_Comment by @not-my-profile on 2023-01-26 03:04_

Rebased :)

---

_Merged by @charliermarsh on 2023-01-26 03:08_

---

_Closed by @charliermarsh on 2023-01-26 03:08_

---

_Renamed from "refactor CLI (1/2): Cosmetic changes" to "refactor CLI (1/3): Cosmetic changes" by @not-my-profile on 2023-01-26 05:55_

---
