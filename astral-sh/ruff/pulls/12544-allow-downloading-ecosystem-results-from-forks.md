```yaml
number: 12544
title: Allow downloading ecosystem results from forks
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: ecosystem-allow-forks
created_at: 2024-07-27T16:27:46Z
updated_at: 2024-07-27T17:57:21Z
url: https://github.com/astral-sh/ruff/pull/12544
synced_at: 2026-01-10T21:47:02Z
```

# Allow downloading ecosystem results from forks

---

_Pull request opened by @MichaReiser on 2024-07-27 16:27_

## Summary

The default for `allow_forks` changed from `true` to `false` with the release of v4 on the 3rd of June.

Fixes https://github.com/astral-sh/ruff/issues/12543

## Test Plan

* [Run](https://github.com/astral-sh/ruff/actions/runs/10124925760/job/27999836193)
* [Comment](https://github.com/astral-sh/ruff/pull/12538#issuecomment-2254193716)


---

_Label `ci` added by @MichaReiser on 2024-07-27 16:31_

---

_@MichaReiser reviewed on 2024-07-27 16:32_

---

_Review comment by @MichaReiser on `.github/workflows/pr-comment.yaml`:6 on 2024-07-27 16:32_

```suggestion
    workflows: [CI]
    types: [completed]
```

---

_@charliermarsh approved on 2024-07-27 16:41_

Thanks boss!

---

_Merged by @MichaReiser on 2024-07-27 17:57_

---

_Closed by @MichaReiser on 2024-07-27 17:57_

---

_Branch deleted on 2024-07-27 17:57_

---
