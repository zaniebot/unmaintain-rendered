```yaml
number: 16105
title: "`uv cache prune --ci` sometimes takes forever on GitHub runners"
type: issue
state: closed
author: eifinger
labels:
  - bug
assignees: []
created_at: 2025-10-02T18:51:41Z
updated_at: 2025-10-06T10:21:51Z
url: https://github.com/astral-sh/uv/issues/16105
synced_at: 2026-01-10T03:23:54Z
```

# `uv cache prune --ci` sometimes takes forever on GitHub runners

---

_Issue opened by @eifinger on 2025-10-02 18:51_

### Summary

In https://github.com/astral-sh/setup-uv/issues/588 users report that when the action executes `uv cache prune --ci` it sometimes never logs/prints anything and never terminates. As far as I can see all existing reports are running on `ubuntu-latest`

UPDATE: This is most likely related to #15888. Two users reporting the issue have a running uv process (webserver running in the background and not properly shut down after tests).

### Platform

GitHub ubuntu-latest

### Version

0.8.22

### Python version

_No response_

---

_Label `bug` added by @eifinger on 2025-10-02 18:51_

---

_Comment by @my1e5 on 2025-10-03 10:34_

Related to https://github.com/astral-sh/uv/pull/15888?

---

_Comment by @konstin on 2025-10-06 10:21_

Tracking as duplicate of https://github.com/astral-sh/uv/issues/16112

---

_Closed by @konstin on 2025-10-06 10:21_

---
