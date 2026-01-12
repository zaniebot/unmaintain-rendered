```yaml
number: 2218
title: Fallback to fresh request on non-validating 304
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
  - registry
assignees: []
merged: true
base: main
head: charlie/304
created_at: 2024-03-05T20:25:08Z
updated_at: 2024-03-06T22:51:04Z
url: https://github.com/astral-sh/uv/pull/2218
synced_at: 2026-01-12T16:04:55Z
```

# Fallback to fresh request on non-validating 304

---

_@charliermarsh_

## Summary

We're seeing reports that Sonatype Nexus isn't working with cached data. Users are reporting 304 responses that show "Found modified response..." path in the logs. I can't reproduce this on latest Sonatype Nexus, but my best guess is that there's a 304 response that is failing our validators, and we try to use that as if it's a "complete" response?

Closes https://github.com/astral-sh/uv/issues/1754.

---

_Marked ready for review by @charliermarsh on 2024-03-05 20:27_

---

_Label `compatibility` added by @charliermarsh on 2024-03-05 20:30_

---

_Label `registry` added by @charliermarsh on 2024-03-05 20:30_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-05 20:30_

---

_Merged by @charliermarsh on 2024-03-06 22:51_

---

_Closed by @charliermarsh on 2024-03-06 22:51_

---

_Branch deleted on 2024-03-06 22:51_

---
