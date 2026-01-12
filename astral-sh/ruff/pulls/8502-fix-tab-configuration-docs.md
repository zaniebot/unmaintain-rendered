```yaml
number: 8502
title: Fix tab configuration docs
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/fix-docs
created_at: 2023-11-06T02:53:56Z
updated_at: 2023-11-09T20:53:06Z
url: https://github.com/astral-sh/ruff/pull/8502
synced_at: 2026-01-12T15:55:26Z
```

# Fix tab configuration docs

---

_@dhruvmanila_

Otherwise it doesn't render as expected. I basically ran `cargo dev generate-all` again.

---

_Label `documentation` added by @dhruvmanila on 2023-11-06 02:53_

---

_Merged by @dhruvmanila on 2023-11-06 03:02_

---

_Closed by @dhruvmanila on 2023-11-06 03:02_

---

_Branch deleted on 2023-11-06 03:02_

---

_Comment by @doolio on 2023-11-08 13:06_

This fix isn't reflected in the docs site. See [here](https://docs.astral.sh/ruff/configuration/#jupyter-notebook-discovery).

---

_Comment by @doolio on 2023-11-09 20:51_

Seems to be reflected now.

---

_Comment by @charliermarsh on 2023-11-09 20:53_

We publish new docs on every release, so changes on main aren’t rolled out until we cut a new version of Ruff. (This is intentional, since we don’t want the docs to e.g. list newly implemented rules that don’t exist in the published version.)

---
