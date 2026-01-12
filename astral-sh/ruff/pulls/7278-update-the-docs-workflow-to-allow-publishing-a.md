```yaml
number: 7278
title: "Update the `docs` workflow to allow publishing a specific ref"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - release
assignees: []
merged: true
base: main
head: zanie/docs-ref
created_at: 2023-09-11T18:21:44Z
updated_at: 2023-09-11T19:12:17Z
url: https://github.com/astral-sh/ruff/pull/7278
synced_at: 2026-01-12T15:55:23Z
```

# Update the `docs` workflow to allow publishing a specific ref

---

_@zanieb_

Related https://github.com/astral-sh/ruff/issues/7276

Our docs publishing action does not allow targetting a specific commit when run manually which means we cannot update the documentation to anything but the latest commit on `main`. This change allows a ref to be provided.

---

_@MichaReiser approved on 2023-09-11 18:23_

---

_@dhruvmanila reviewed on 2023-09-11 18:29_

---

_Review comment by @dhruvmanila on `.github/workflows/docs.yaml`:22 on 2023-09-11 18:29_

```suggestion
          ref: ${{ inputs.ref }}
```

---

_Label `internal` added by @zanieb on 2023-09-11 18:39_

---

_Merged by @zanieb on 2023-09-11 18:51_

---

_Closed by @zanieb on 2023-09-11 18:51_

---

_Branch deleted on 2023-09-11 18:51_

---

_Label `release` added by @zanieb on 2023-09-11 19:12_

---
