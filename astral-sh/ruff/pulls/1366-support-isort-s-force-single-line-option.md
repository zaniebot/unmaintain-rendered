```yaml
number: 1366
title: "Support isort's force-single-line option"
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: isort-force-single-line
created_at: 2022-12-24T20:22:54Z
updated_at: 2022-12-27T13:52:04Z
url: https://github.com/astral-sh/ruff/pull/1366
synced_at: 2026-01-12T15:55:06Z
```

# Support isort's force-single-line option

---

_@squiddy_

Forces all from imports to appear on their own line.

Closes #1252

---

_@charliermarsh reviewed on 2022-12-24 21:16_

---

_Review comment by @charliermarsh on `src/isort/format.rs`:86 on 2022-12-24 21:16_

I wonder if we could instead model this as an additional transform from `OrderedImportBlock` to `OrderedImportBlock`, and not change these format functions. Something like `split_imports`?

---

_Review comment by @squiddy on `src/isort/format.rs`:86 on 2022-12-24 21:30_

Interesting, let me look into that.

---

_@squiddy reviewed on 2022-12-24 21:30_

---

_Converted to draft by @squiddy on 2022-12-25 15:53_

---

_Marked ready for review by @squiddy on 2022-12-27 09:23_

---

_Merged by @charliermarsh on 2022-12-27 13:51_

---

_Closed by @charliermarsh on 2022-12-27 13:51_

---

_Branch deleted on 2022-12-27 13:52_

---
