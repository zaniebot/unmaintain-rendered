```yaml
number: 7568
title: "ci(docker): improve release tagging order and display on ghcr.io"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: docker-release-improvements
created_at: 2024-09-20T01:25:09Z
updated_at: 2024-09-20T21:55:25Z
url: https://github.com/astral-sh/uv/pull/7568
synced_at: 2026-01-12T16:07:53Z
```

# ci(docker): improve release tagging order and display on ghcr.io

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/7458

This change adds a new job at the end of docker publish which adds annotations the primary image digests in order to make ghcr.io rank `ghcr.io/astral-sh/uv` at the top once again. The solution is to annotate the index at the end during a re-publish to make ghcr.io consider it a more updated entry than the others and rank it at the top once again.

## Test Plan

Tested on release run on my own fork
* Packages: https://github.com/samypr100/uv/pkgs/container/uv will show `ghcr.io/astral-sh/uv` first once again
* Run: https://github.com/samypr100/uv/actions/runs/10951404736


---

_Marked ready for review by @samypr100 on 2024-09-20 01:29_

---

_Comment by @samypr100 on 2024-09-20 01:30_

CC @zanieb 

---

_@zanieb approved on 2024-09-20 19:05_

Thank you! I'm impressed you figured something out here. This is kind of wild, but not sure what else to do.

---

_Label `releases` added by @zanieb on 2024-09-20 19:05_

---

_Merged by @zanieb on 2024-09-20 19:05_

---

_Closed by @zanieb on 2024-09-20 19:05_

---

_Branch deleted on 2024-09-20 21:55_

---
