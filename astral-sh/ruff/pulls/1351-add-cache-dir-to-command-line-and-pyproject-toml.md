```yaml
number: 1351
title: Add cache-dir to command-line and pyproject.toml
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: cache-dir
created_at: 2022-12-23T14:46:52Z
updated_at: 2022-12-27T05:43:10Z
url: https://github.com/astral-sh/ruff/pull/1351
synced_at: 2026-01-12T05:36:31Z
```

# Add cache-dir to command-line and pyproject.toml

---

_Pull request opened by @squiddy on 2022-12-23 14:46_

Refs #1313

---

_Review comment by @squiddy on `pyproject.toml`:36 on 2022-12-23 15:22_

Left over from testing. Will remove it 

---

_@charliermarsh reviewed on 2022-12-23 16:38_

---

_Review comment by @charliermarsh on `src/main.rs`:146 on 2022-12-23 16:38_

I think we may need to call `cache::init` for every `Settings` object now, since different files could end up using different cache directories.

---

_@squiddy reviewed on 2022-12-23 19:04_

---

_Review comment by @squiddy on `src/main.rs`:146 on 2022-12-23 19:05_



Not sure how to go about this. Was thinking at first to do that in commands::run before running lint_path. But not only would that then happen per file, it can also happen at the same time due to par_iter.

My only other idea is to initialize the cache at the beginning for all resolved settings, keeping track of that and checking that per file.


---

_@squiddy reviewed on 2022-12-23 19:05_

---

_Review comment by @charliermarsh on `src/main.rs`:146 on 2022-12-23 19:21_

I think the latter option probably makes sense, to initialize for all resolved settings once we’ve gathered them up.

---

_@charliermarsh reviewed on 2022-12-23 19:21_

---

_@charliermarsh reviewed on 2022-12-23 19:21_

---

_Review comment by @charliermarsh on `src/main.rs`:146 on 2022-12-23 19:21_

Conceptually it’d be nice to just do it in the cache setter (like, right before we write), but maybe that’s hard to get right in a multi-threaded context?

---

_Merged by @charliermarsh on 2022-12-24 03:58_

---

_Closed by @charliermarsh on 2022-12-24 03:58_

---

_Branch deleted on 2022-12-27 05:43_

---
