```yaml
number: 13719
title: Disallow arm64 Windows Python installs for now
type: pull_request
state: closed
author: geofft
labels:
  - windows
assignees: []
base: main
head: skip-arm64-windows
created_at: 2025-05-29T17:11:53Z
updated_at: 2025-06-13T17:21:27Z
url: https://github.com/astral-sh/uv/pull/13719
synced_at: 2026-01-10T11:10:42Z
```

# Disallow arm64 Windows Python installs for now

---

_Pull request opened by @geofft on 2025-05-29 17:11_

See #12906 and astral-sh/python-build-standalone#387 - as soon as we start shipping arm64 Windows Python builds in python-build-standalone, uv is going to start discovering them, and there aren't enough arm64 Windows wheels for that to make sense as a default yet. As a very minimal stopgap, consider those builds incompatible.

No functional change expected at the moment, but this unblocks python-build-standalone shipping arm64 Windows Python builds and lets us manage the transition in some better way like a configuration knob.

---

_Label `windows` added by @geofft on 2025-05-29 17:12_

---

_Assigned to @zanieb by @zanieb on 2025-05-29 17:18_

---

_@Gankra reviewed on 2025-05-29 19:08_

Absolutely brutal

---

_Comment by @zanieb on 2025-05-29 19:58_

I don't think this is quite right, won't we ignore any arm64 Python installations on the machine now? I'll look more closely later.

---

_Comment by @Gankra on 2025-05-29 22:18_

My alternative solution is here:

* https://github.com/astral-sh/uv/pull/13724

---

_Comment by @geofft on 2025-06-13 17:21_

All right, @Gankra has been thinking harder about how to implement this well, so closing this in favor of her changes :)

---

_Closed by @geofft on 2025-06-13 17:21_

---
