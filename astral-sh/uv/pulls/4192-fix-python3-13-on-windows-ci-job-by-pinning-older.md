```yaml
number: 4192
title: Fix python3.13 on windows CI job by pinning older version
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-check-3.13-on-windows
created_at: 2024-06-10T11:31:58Z
updated_at: 2024-06-10T11:49:25Z
url: https://github.com/astral-sh/uv/pull/4192
synced_at: 2026-01-10T13:54:02Z
```

# Fix python3.13 on windows CI job by pinning older version

---

_Pull request opened by @konstin on 2024-06-10 11:31_

See https://github.com/actions/setup-python/issues/888

---

_Label `internal` added by @konstin on 2024-06-10 11:31_

---

_Review requested from @AlexWaygood by @konstin on 2024-06-10 11:31_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yml`:901 on 2024-06-10 11:32_

I think this is what you need:

```suggestion
          python-version: "3.13.0-beta.1"
```

---

_@AlexWaygood reviewed on 2024-06-10 11:32_

---

_@AlexWaygood reviewed on 2024-06-10 11:35_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yml`:901 on 2024-06-10 11:35_

(The reason is that it needs to match one of the versions in the manifest file at https://github.com/actions/python-versions/blob/main/versions-manifest.json)

---

_@AlexWaygood approved on 2024-06-10 11:40_

LGTM (could possibly add a TODO comment to unpin once it's resolved and link to the setup-python issue)

---

_Marked ready for review by @konstin on 2024-06-10 11:43_

---

_Merged by @konstin on 2024-06-10 11:49_

---

_Closed by @konstin on 2024-06-10 11:49_

---

_Branch deleted on 2024-06-10 11:49_

---
