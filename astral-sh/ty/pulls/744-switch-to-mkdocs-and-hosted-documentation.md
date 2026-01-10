```yaml
number: 744
title: Switch to MkDocs and hosted documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: feat/docs
created_at: 2025-07-01T16:13:55Z
updated_at: 2025-07-02T14:41:43Z
url: https://github.com/astral-sh/ty/pull/744
synced_at: 2026-01-10T02:34:10Z
```

# Switch to MkDocs and hosted documentation

---

_Pull request opened by @zanieb on 2025-07-01 16:13_

This is a tracking branch for moving from GitHub markdown documentation to MkDocs and hosted documentation on `docs.astral.sh`.

It includes the following changes:

- #729 
- #734 
- #731 
- #746 
- #747 
- #748 
- #752 

And some to-do tasks are:

- [x] Update the GitHub admonitions to MkDocs formatting
- [x] Organize the documentation into new sections
- [x] Revisit generation of reference documentation (see https://github.com/astral-sh/ty/pull/292)
  - [x] Fix the rules reference generation (the `<details>` sections don't look good)
- [x] Update the docs links in the repo-root `README.md`
- [ ] Update https://ty.dev/rules to point to the new rules-reference page, not to https://github.com/astral-sh/ty/blob/main/docs/reference/rules.md

Post-merge to-do tasks:

- [ ] Consider writing guides for core workflows
- [ ] Consider using `pre-commit` instead of the `prettier` format command (adapt `CONTRIBUTING.md`)

The following changes are part of this task but can be merged to `main` prior to this feature:

- #730 

---

_Label `documentation` added by @zanieb on 2025-07-01 16:13_

---

_Marked ready for review by @sharkdp on 2025-07-02 14:33_

---

_@sharkdp approved on 2025-07-02 14:33_

---

_Merged by @zanieb on 2025-07-02 14:40_

---

_Closed by @zanieb on 2025-07-02 14:40_

---

_Branch deleted on 2025-07-02 14:40_

---
