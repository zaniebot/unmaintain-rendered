```yaml
number: 22766
title: Drop PPC64 builds
type: pull_request
state: open
author: konstin
labels:
  - breaking
assignees: []
base: main
head: konsti/drop-ppc64
created_at: 2026-01-20T15:15:23Z
updated_at: 2026-01-21T08:29:10Z
url: https://github.com/astral-sh/ruff/pull/22766
synced_at: 2026-01-21T09:03:02Z
```

# Drop PPC64 builds

---

_@konstin_

PPC64 seems dead, and it's only supported on one exact manylinux version (https://github.com/pypa/auditwheel/issues/669), so we should drop it.

It's arguable whether this is a breaking change, since it doesn't remove support for the platform (which is already barely support by manylinux), but it does make installation more complex on PPC64.

---

_@ntBre approved on 2026-01-20 15:56_

Thank you! I'd probably lean toward considering this a breaking change since we have a minor release coming up anyway, but I'm also happy to land it sooner if you prefer.

---

_Label `breaking` added by @ntBre on 2026-01-20 15:56_

---

_Added to milestone `v0.15` by @ntBre on 2026-01-20 15:56_

---

_@MichaReiser approved on 2026-01-21 08:29_

Shipping it as part of 0.15 makes sense to me. 

I tried to find out why ppc64 support was added initially, but all I could find was https://github.com/astral-sh/ruff/pull/45, which doesn't mention a reason. 

It seems easy enough to add them again if users miss them and we then at least know why we need to support them going forward. 

I double checked that ruff-vscode doesn't publish a PPC64 extension. So this looks good from my end

---
