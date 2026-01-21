```yaml
number: 2571
title: Drop PPC64 builds
type: pull_request
state: merged
author: konstin
labels:
  - release
assignees: []
merged: true
base: main
head: konsti/drop-ppc64
created_at: 2026-01-20T15:16:15Z
updated_at: 2026-01-21T08:31:42Z
url: https://github.com/astral-sh/ty/pull/2571
synced_at: 2026-01-21T09:02:46Z
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

_Comment by @MichaReiser on 2026-01-20 20:16_

Hmm. I don't have much context myself but I'll try to find the PR that added support for it in Ruff to understand what the motivation was back then.

---

_Comment by @MichaReiser on 2026-01-21 08:31_

I couldn't find a reason why PPC64 support was added in Ruff. Given that it's very easy for us to restore PPC64 support if users ask for it, I suggest we go ahead and merge this. This will at least give us signal on why we have to support PPC64 going forward (and for how long).

---

_Label `release` added by @MichaReiser on 2026-01-21 08:31_

---

_Merged by @MichaReiser on 2026-01-21 08:31_

---

_Closed by @MichaReiser on 2026-01-21 08:31_

---

_Branch deleted on 2026-01-21 08:31_

---
