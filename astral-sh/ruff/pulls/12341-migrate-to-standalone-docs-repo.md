```yaml
number: 12341
title: Migrate to standalone docs repo
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs-repo
created_at: 2024-07-16T01:45:12Z
updated_at: 2024-07-18T15:35:51Z
url: https://github.com/astral-sh/ruff/pull/12341
synced_at: 2026-01-12T15:55:40Z
```

# Migrate to standalone docs repo

---

_@charliermarsh_

## Summary

See: https://github.com/astral-sh/uv/pull/5081


---

_Label `documentation` added by @charliermarsh on 2024-07-16 01:45_

---

_Review comment by @MichaReiser on `.github/workflows/publish-docs.yml`:94 on 2024-07-16 10:01_

```suggestion
      - name: "Clone docs repo"
```

---

_@MichaReiser approved on 2024-07-16 10:03_

---

_Comment by @MichaReiser on 2024-07-16 10:03_

Does this need any update to the contribution guidelines? What's the new workflow for making changes to the documentation?

---

_Comment by @charliermarsh on 2024-07-16 13:08_

Nope! No changes to anything at all. This only affects the publishing pipeline which happens during deploy.

---

_Comment by @charliermarsh on 2024-07-16 13:22_

I won't be merging this until I change a few other things downstream though (and we feel ready to post the uv docs).

---

_Comment by @dhruvmanila on 2024-07-18 15:31_

(Merging this as requested by Charlie.)

---

_Merged by @dhruvmanila on 2024-07-18 15:35_

---

_Closed by @dhruvmanila on 2024-07-18 15:35_

---

_Branch deleted on 2024-07-18 15:35_

---
