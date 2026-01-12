```yaml
number: 8545
title: Update pre-commit documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pre-commit
created_at: 2023-11-07T16:29:39Z
updated_at: 2023-11-08T17:07:28Z
url: https://github.com/astral-sh/ruff/pull/8545
synced_at: 2026-01-10T23:40:55Z
```

# Update pre-commit documentation

---

_Pull request opened by @charliermarsh on 2023-11-07 16:29_

I got some feedback on Mastodon that it wasn't clear how to use the linter and formatter together in pre-commit (mostly in the pre-commit repo's documentation, which is even less clear, but the two should be consistent).

---

_Label `documentation` added by @charliermarsh on 2023-11-07 16:29_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-07 16:29_

---

_@zanieb approved on 2023-11-07 16:32_

---

_@charliermarsh reviewed on 2023-11-07 16:36_

---

_Review comment by @charliermarsh on `docs/integrations.md`:57 on 2023-11-07 16:36_

@zanieb -- I'm a little nervous about this change, is there any scenario in which this would be wrong?

---

_@zanieb reviewed on 2023-11-07 16:42_

---

_Review comment by @zanieb on `docs/integrations.md`:57 on 2023-11-07 16:42_

Well.. if you're using format rules? Like `E501` would have false positives if you ran it before Black.

---

_@charliermarsh reviewed on 2023-11-07 17:11_

---

_Review comment by @charliermarsh on `docs/integrations.md`:57 on 2023-11-07 17:11_

But wouldn't Black then cause file changes, which would require you to re-run pre-commit? Or are there modes where people run pre-commit with allowed changes?

---

_@zanieb reviewed on 2023-11-07 17:12_

---

_Review comment by @zanieb on `docs/integrations.md`:57 on 2023-11-07 17:12_

Ruff would fail first, Black wouldn't run.

---

_Merged by @charliermarsh on 2023-11-07 18:40_

---

_Closed by @charliermarsh on 2023-11-07 18:40_

---

_Branch deleted on 2023-11-07 18:40_

---

_Comment by @zanieb on 2023-11-08 17:07_

#8567 adds auto-update of these versions

---
