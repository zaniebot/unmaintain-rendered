```yaml
number: 2571
title: Drop PPC64 builds
type: pull_request
state: open
author: konstin
labels: []
assignees: []
base: main
head: konsti/drop-ppc64
created_at: 2026-01-20T15:16:15Z
updated_at: 2026-01-20T19:22:56Z
url: https://github.com/astral-sh/ty/pull/2571
synced_at: 2026-01-20T20:43:14Z
```

# Drop PPC64 builds

---

_@konstin_

PPC64 seems dead, and it's only supported on one exact manylinux version (https://github.com/pypa/auditwheel/issues/669), so we should drop it.

It's arguable whether this is a breaking change, since it doesn't remove support for the platform (which is already barely support by manylinux), but it does make installation more complex on PPC64.

---

_Comment by @carljm on 2026-01-20 19:22_

I don't have any objection to this, but I'm also not sure I have the right context to make the decision. I'll approve to unblock, and trust you to request reviews from anyone who should have a chance to review (maybe @MichaReiser ?)

---

_@carljm approved on 2026-01-20 19:22_

---
