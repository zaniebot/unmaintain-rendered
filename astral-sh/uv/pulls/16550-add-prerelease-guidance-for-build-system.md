```yaml
number: 16550
title: Add prerelease guidance for build-system resolution failures
type: pull_request
state: merged
author: terror
labels:
  - error messages
assignees: []
merged: true
base: main
head: prelease-build-system
created_at: 2025-11-02T04:22:28Z
updated_at: 2025-11-02T20:03:46Z
url: https://github.com/astral-sh/uv/pull/16550
synced_at: 2026-01-12T16:12:19Z
```

# Add prerelease guidance for build-system resolution failures

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16496

This PR updates the resolver so `build-system` dependency failures surface prerelease hints even when prerelease selection is fixed. When a build dependency only has prerelease candidates, or the requested version explicitly includes a prerelease marker, we now emit a tailored hint explaining that build environments can’t auto-enable prereleases and describing how to opt in. 

---

_@charliermarsh approved on 2025-11-02 18:23_

Nice, thanks!

---

_Label `error messages` added by @charliermarsh on 2025-11-02 18:26_

---

_Merged by @charliermarsh on 2025-11-02 18:38_

---

_Closed by @charliermarsh on 2025-11-02 18:38_

---

_Branch deleted on 2025-11-02 20:03_

---
