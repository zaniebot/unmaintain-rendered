```yaml
number: 1644
title: "Implement flake8-bandit rule `S108`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: bandit-s108
created_at: 2023-01-04T23:52:27Z
updated_at: 2023-01-05T02:13:39Z
url: https://github.com/astral-sh/ruff/pull/1644
synced_at: 2026-01-12T05:36:32Z
```

# Implement flake8-bandit rule `S108`

---

_Pull request opened by @edgarrmondragon on 2023-01-04 23:52_

Releated: #1646 

---

_Marked ready for review by @edgarrmondragon on 2023-01-05 00:57_

---

_Review comment by @charliermarsh on `src/flake8_bandit/settings.rs`:41 on 2023-01-05 01:00_

You could consider just having `hardcoded_tmp_directory` (removing `hardcoded_tmp_directory_extend` from `Settings`), and then concatenating the two in `From<Options>`.

---

_Review comment by @charliermarsh on `src/flake8_bandit/mod.rs`:24 on 2023-01-05 01:01_

Looks reasonable to me. For other plugins, we tend to use the default, and create dedicated test-cases for overridden settings. But this is cool too.

---

_@charliermarsh approved on 2023-01-05 01:01_

---

_@edgarrmondragon reviewed on 2023-01-05 02:05_

---

_Review comment by @edgarrmondragon on `src/flake8_bandit/mod.rs`:24 on 2023-01-05 02:05_

Thanks for the feedback. These were a bit unreadable so I refactored them.

---

_@edgarrmondragon reviewed on 2023-01-05 02:05_

---

_Review comment by @edgarrmondragon on `src/flake8_bandit/settings.rs`:41 on 2023-01-05 02:05_

Yeah, that makes sense.

---

_Merged by @charliermarsh on 2023-01-05 02:11_

---

_Closed by @charliermarsh on 2023-01-05 02:11_

---

_Branch deleted on 2023-01-05 02:13_

---
