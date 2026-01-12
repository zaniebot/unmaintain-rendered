```yaml
number: 11157
title: Bump alpine default tag to 3.21
type: pull_request
state: merged
author: samypr100
labels:
  - breaking
  - releases
assignees: []
merged: true
base: tracking/060
head: update-alpine-docker-tag
created_at: 2025-02-02T00:10:15Z
updated_at: 2025-02-08T15:41:16Z
url: https://github.com/astral-sh/uv/pull/11157
synced_at: 2026-01-12T16:09:42Z
```

# Bump alpine default tag to 3.21

---

_@samypr100_

## Summary

Alpine 3.21 has been released for a few months and it's now being used officially under `alpine` based [python images](https://hub.docker.com/_/python), hence our python-alpine based images has been using 3.21 since uv 0.5.8 under the hood.

This could arguably be `breaking` as we're dropping alpine3.20 top-level tag, so it could be a good candidate for 0.6.0.

Alternatively, we can keep support for 3.20 and make this non-breaking by simply repointing alpine to now be 3.21 and keeping the 3.20 tag around.


---

_Added to milestone `v0.6.0` by @konstin on 2025-02-02 11:00_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-05 14:41_

---

_Review requested from @konstin by @charliermarsh on 2025-02-05 14:41_

---

_Label `releases` added by @charliermarsh on 2025-02-05 14:41_

---

_Label `breaking` added by @charliermarsh on 2025-02-05 14:41_

---

_@zanieb approved on 2025-02-05 14:51_

Just waiting for 0.6

---

_@konstin approved on 2025-02-06 10:04_

lgtm

---

_Merged by @zanieb on 2025-02-07 20:44_

---

_Closed by @zanieb on 2025-02-07 20:44_

---

_Branch deleted on 2025-02-08 15:41_

---
