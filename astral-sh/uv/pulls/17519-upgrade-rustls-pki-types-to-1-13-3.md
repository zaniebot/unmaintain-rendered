```yaml
number: 17519
title: Upgrade rustls-pki-types to 1.13.3
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
base: main
head: claude/upgrade-rustls-pki-types-W3sVa
created_at: 2026-01-16T14:12:38Z
updated_at: 2026-01-17T16:00:33Z
url: https://github.com/astral-sh/uv/pull/17519
synced_at: 2026-01-17T16:15:32Z
```

# Upgrade rustls-pki-types to 1.13.3

---

_@zanieb_

Pulls the upstream fix for #17494 (https://github.com/rustls/pki-types/releases/tag/v%2F1.13.3)

Most other significant changes are in https://github.com/rustls/pki-types/releases/tag/v%2F1.13.0

---

_Marked ready for review by @zanieb on 2026-01-16 17:20_

---

_Comment by @samypr100 on 2026-01-17 05:08_

Should the changes from https://github.com/astral-sh/uv/pull/17503 be reverted?

---

_Comment by @zanieb on 2026-01-17 16:00_

I don't think so, we were already doing an existence check and the cost of checking if it's a directory is low and I presume it gives a better error message.

---
