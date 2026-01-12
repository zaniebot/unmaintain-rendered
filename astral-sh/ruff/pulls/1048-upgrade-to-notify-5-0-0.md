```yaml
number: 1048
title: Upgrade to notify 5.0.0
type: pull_request
state: merged
author: messense
labels: []
assignees: []
merged: true
base: main
head: notify-5
created_at: 2022-12-05T04:55:32Z
updated_at: 2022-12-05T15:00:09Z
url: https://github.com/astral-sh/ruff/pull/1048
synced_at: 2026-01-12T15:55:05Z
```

# Upgrade to notify 5.0.0

---

_@messense_

Changelog: https://github.com/notify-rs/notify/blob/main/CHANGELOG.md#notify-500-2022-08-28

---

_@messense reviewed on 2022-12-05 05:00_

---

_Review comment by @messense on `src/main.rs`:349 on 2022-12-05 05:00_

Maybe the list of paths should be passed into `run_once` instead? Or we should check `.py` in the list of paths and only run `run_once` once? I think the later preserves the previous behavior.

I'm not totally sure, the current state is the minimal changes to make it compile.

---

_Merged by @charliermarsh on 2022-12-05 14:58_

---

_Closed by @charliermarsh on 2022-12-05 14:58_

---

_Branch deleted on 2022-12-05 15:00_

---
