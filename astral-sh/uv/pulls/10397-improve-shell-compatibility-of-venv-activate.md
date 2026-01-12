```yaml
number: 10397
title: improve shell compatibility of venv activate scripts
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/shellcheck
created_at: 2025-01-08T15:13:41Z
updated_at: 2025-01-11T01:12:23Z
url: https://github.com/astral-sh/uv/pull/10397
synced_at: 2026-01-12T16:09:16Z
```

# improve shell compatibility of venv activate scripts

---

_@Gankra_

The shellcheck action we uses misses some files, so they fell out of spec for what we support. This PR first and foremost adds them to the scanning list, and then fixes the issues found.

Fixes #7480

---

_Marked ready for review by @Gankra on 2025-01-08 17:46_

---

_Label `bug` added by @zanieb on 2025-01-08 17:51_

---

_@zanieb approved on 2025-01-08 17:51_

---

_Merged by @Gankra on 2025-01-08 18:12_

---

_Closed by @Gankra on 2025-01-08 18:12_

---

_Branch deleted on 2025-01-08 18:12_

---

_Comment by @andypost on 2025-01-11 00:53_

This change caused our CI to fail using `alpine:edge` image 

```
web-1  | /srv/scripts/run.sh: /srv/.venv/bin/activate: line 81: OSTYPE: parameter not set
```

---

_Comment by @andypost on 2025-01-11 01:12_

Claude said to add default
```sh
# Set default OSTYPE if not set
: "${OSTYPE:=unknown}"
```

---
