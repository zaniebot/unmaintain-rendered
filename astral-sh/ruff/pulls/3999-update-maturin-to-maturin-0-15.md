```yaml
number: 3999
title: Update maturin to maturin 0.15
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: update_maturin
created_at: 2023-04-17T16:46:21Z
updated_at: 2023-05-12T13:43:08Z
url: https://github.com/astral-sh/ruff/pull/3999
synced_at: 2026-01-12T15:55:14Z
```

# Update maturin to maturin 0.15

---

_@konstin_

This allows removing the deprecated `[package.metadata.maturin]`

See https://github.com/charliermarsh/ruff/pull/3998#discussion_r1168985807

---

_Review requested from @charliermarsh by @konstin on 2023-04-17 16:46_

---

_Review requested from @MichaReiser by @konstin on 2023-04-17 16:46_

---

_Comment by @github-actions[bot] on 2023-04-17 17:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-04-17 20:08_

---

_Converted to draft by @konstin on 2023-04-18 16:11_

---

_Comment by @konstin on 2023-04-18 16:11_

converting this to draft to avoid accidental to early merging 

---

_Renamed from "Update maturin to maturin>=0.14.17" to "Update maturin to maturin 0.15" by @konstin on 2023-05-10 15:36_

---

_Review requested from @charliermarsh by @konstin on 2023-05-10 15:37_

---

_Comment by @konstin on 2023-05-10 15:37_

Updated this to update to maturin 0.15 instead, i'd merge this now if you both sign off

---

_@charliermarsh approved on 2023-05-10 15:37_

---

_Marked ready for review by @konstin on 2023-05-10 15:38_

---

_Review comment by @Skylion007 on `crates/flake8_to_ruff/pyproject.toml`:29 on 2023-05-11 13:12_

I am personally against max version constraints. See this blogpost for good arguments about why they are not useful, and actually detrimental: 
https://iscinumpy.dev/post/bound-version-constraints/

---

_@Skylion007 reviewed on 2023-05-11 13:12_

---

_Review comment by @konstin on `crates/flake8_to_ruff/pyproject.toml`:29 on 2023-05-11 15:22_

Note that this is not a dependency in the classical sense, but this is the build system. Without upper bounding this, packages published with previous maturin version would become unbuildable once we publish a maturin version with breaking changes

---

_@konstin reviewed on 2023-05-11 15:22_

---

_Merged by @konstin on 2023-05-12 13:43_

---

_Closed by @konstin on 2023-05-12 13:43_

---

_Branch deleted on 2023-05-12 13:43_

---
