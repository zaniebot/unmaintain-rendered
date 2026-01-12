```yaml
number: 12188
title: Replace change detection GitHub Action
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/change
created_at: 2025-03-15T15:50:46Z
updated_at: 2025-03-16T15:41:23Z
url: https://github.com/astral-sh/uv/pull/12188
synced_at: 2026-01-12T16:10:09Z
```

# Replace change detection GitHub Action

---

_@charliermarsh_

## Summary

`tj-actions/changed-files` no longer exists due to a malicious commit. This PR replaces it with a minimal shell script to get us unblocked.


---

_Review requested from @zanieb by @charliermarsh on 2025-03-15 16:05_

---

_Marked ready for review by @charliermarsh on 2025-03-15 16:05_

---

_Label `testing` added by @charliermarsh on 2025-03-15 16:06_

---

_Label `testing` removed by @charliermarsh on 2025-03-15 16:06_

---

_Label `internal` added by @charliermarsh on 2025-03-15 16:06_

---

_Renamed from "Replace change detection" to "Replace change detection GitHub Action" by @charliermarsh on 2025-03-15 16:08_

---

_@zanieb reviewed on 2025-03-15 16:26_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:43 on 2025-03-15 16:26_

These backticks are going to evaluate

```
❯ echo "foo `bar`"
zsh: command not found: bar
foo 
```

---

_Review requested from @zanieb by @charliermarsh on 2025-03-15 16:43_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:42 on 2025-03-15 16:44_

I'd use `"${file}"` throughout — I don't think there's an attack vector here because of the way you're iterating but it's safer.

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:67 on 2025-03-15 16:45_

```suggestion
          done <<< "${CHANGED_FILES}"
```

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:68 on 2025-03-15 16:45_

```suggestion
          echo "code_any_changed=${CODE_CHANGED}" >> "$GITHUB_OUTPUT"
```

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:43 on 2025-03-15 16:47_

It might be worth retaining the comment on why we run tests on these files

---

_@zanieb approved on 2025-03-15 16:48_

Thanks!

---

_Merged by @charliermarsh on 2025-03-15 17:12_

---

_Closed by @charliermarsh on 2025-03-15 17:12_

---

_Branch deleted on 2025-03-15 17:12_

---

_Comment by @vladyslav-burylov on 2025-03-16 15:27_

Hi, @charliermarsh. First of all, thank you for the above remediating security risk quickly. 

Could you please kindly comment if you have plans to disclose some sort of post-mortem outlining potential impact on uv users? Are there any actions which should be done on user side to remediate impact of potentially leaked uv CI secrets?

---

_Comment by @charliermarsh on 2025-03-16 15:40_

Good question: There's no action required on the user side and no impact to our users. We rotated all secrets in the repository, and no secrets that facilitate PyPI publishing were exposed since we use Trusted Publishing.

---

_Comment by @vladyslav-burylov on 2025-03-16 15:41_

Good! Thank you so much for quick response!

---
