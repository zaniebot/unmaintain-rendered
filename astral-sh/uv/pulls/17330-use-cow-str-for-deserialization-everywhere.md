```yaml
number: 17330
title: "Use Cow<str> for deserialization everywhere"
type: pull_request
state: merged
author: woodruffw
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: ww/more-cow
created_at: 2026-01-05T21:11:14Z
updated_at: 2026-01-05T22:57:37Z
url: https://github.com/astral-sh/uv/pull/17330
synced_at: 2026-01-12T16:12:44Z
```

# Use Cow<str> for deserialization everywhere

---

_@woodruffw_

## Summary

Switches all `String::deserialize` callsites to `<Cow<'_, &str>>` for opportunistic borrowing. This should be advantageous for us in cases where we immediately discard the `String` anyways, e.g. any sort of `parse(s: &str) -> T` call.

See #17327 and onwards for context.

## Test Plan

NFC.

---

_Review requested from @zanieb by @woodruffw on 2026-01-05 21:11_

---

_Assigned to @woodruffw by @woodruffw on 2026-01-05 21:11_

---

_Label `performance` added by @woodruffw on 2026-01-05 21:11_

---

_Label `internal` added by @woodruffw on 2026-01-05 21:11_

---

_Review requested from @charliermarsh by @woodruffw on 2026-01-05 22:48_

---

_@charliermarsh reviewed on 2026-01-05 22:53_

---

_Review comment by @charliermarsh on `crates/uv-build-backend/src/metadata.rs`:130 on 2026-01-05 22:53_

This seems marginal since we allocate anyway? (Maybe we should use ArcStr for the given name.)

---

_@charliermarsh approved on 2026-01-05 22:53_

---

_@woodruffw reviewed on 2026-01-05 22:57_

---

_Review comment by @woodruffw on `crates/uv-build-backend/src/metadata.rs`:130 on 2026-01-05 22:57_

Yeah, this one is marginal. I'll look at `ArcStr` in a follow-up 🙂 

---

_Merged by @woodruffw on 2026-01-05 22:57_

---

_Closed by @woodruffw on 2026-01-05 22:57_

---

_Branch deleted on 2026-01-05 22:57_

---
