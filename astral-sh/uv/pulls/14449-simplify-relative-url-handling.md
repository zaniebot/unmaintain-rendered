```yaml
number: 14449
title: Simplify relative URL handling
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/deduplicate-relative-url
created_at: 2025-07-03T21:21:53Z
updated_at: 2025-07-09T23:20:03Z
url: https://github.com/astral-sh/uv/pull/14449
synced_at: 2026-01-12T16:11:13Z
```

# Simplify relative URL handling

---

_@konstin_

I was trying to figure out what the correct relative-and-absolute URL handling function was and realized there are two and they are redundant.

---

_Review requested from @BurntSushi by @konstin on 2025-07-03 21:21_

---

_Label `internal` added by @konstin on 2025-07-03 21:21_

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/base_url.rs`:20 on 2025-07-08 13:12_

Oh nice, okay, I see this logic is in `FileLocation::to_url`.

---

_@BurntSushi approved on 2025-07-08 13:13_

Nice simplification!

---

_Merged by @konstin on 2025-07-09 23:20_

---

_Closed by @konstin on 2025-07-09 23:20_

---

_Branch deleted on 2025-07-09 23:20_

---
