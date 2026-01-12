```yaml
number: 1288
title: Rename PDV checks to PD
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pdv
created_at: 2022-12-19T05:20:07Z
updated_at: 2022-12-19T05:20:29Z
url: https://github.com/astral-sh/ruff/pull/1288
synced_at: 2026-01-12T05:36:31Z
```

# Rename PDV checks to PD

---

_Pull request opened by @charliermarsh on 2022-12-19 05:20_

I can't find any other checks that use the `PD` prefix, apart from https://github.com/pandas-dev/pandas-dev-flaker, which is an internal-only linter for Pandas itself, and uses `PDF`.

(Note that `PDV` will continue to work, but will issue a warning.)

Resolves #1283.


---

_Merged by @charliermarsh on 2022-12-19 05:20_

---

_Closed by @charliermarsh on 2022-12-19 05:20_

---

_Branch deleted on 2022-12-19 05:20_

---
