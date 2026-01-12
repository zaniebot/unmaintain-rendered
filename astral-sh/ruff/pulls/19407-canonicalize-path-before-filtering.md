```yaml
number: 19407
title: Canonicalize path before filtering
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/macos-filter
created_at: 2025-07-17T17:37:35Z
updated_at: 2025-07-17T18:02:18Z
url: https://github.com/astral-sh/ruff/pull/19407
synced_at: 2026-01-12T15:56:39Z
```

# Canonicalize path before filtering

---

_@ntBre_

## Summary

This came up on [Discord](https://discord.com/channels/1039017663004942429/1343692072921731082/1395447082520678440) and also in #19387, but on macOS the tmp directory is a symlink to `/private/tmp`, which breaks this filter. I'm still not quite sure why only these tests are affected when we use the `tempdir_filter` elsewhere, but hopefully this fixes the immediate issue. Just `tempdir.path().canonicalize()` also worked, but I used `dunce` since that's what I saw in other tests (I guess it's not _just_ these tests).

Some related links from uv:
- https://github.com/astral-sh/uv/blob/1b2f212e8b2f91069b858cb7f5905589c9d15add/crates/uv/tests/it/common/mod.rs#L1161-L1178
- https://github.com/astral-sh/uv/blob/1b2f212e8b2f91069b858cb7f5905589c9d15add/crates/uv/tests/it/common/mod.rs#L424-L438
- https://github.com/astral-sh/uv/pull/14290

Thanks to @zanieb for those!

## Test Plan

I tested the `main` branch on my MacBook and reproduced the test failure, then confirmed that the tests pass after the change. Now to make sure it passes on Windows, which caused most of the trouble in the first PR!


---

_Label `internal` added by @ntBre on 2025-07-17 17:38_

---

_@MichaReiser approved on 2025-07-17 17:41_

---

_Comment by @ntBre on 2025-07-17 17:43_

Yeah this is basically what I was afraid of with Windows. I'll try just including both filters and then bust out the Windows VM too if I have to.

---

_Comment by @github-actions[bot] on 2025-07-17 17:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-07-17 18:02_

---

_Closed by @ntBre on 2025-07-17 18:02_

---

_Branch deleted on 2025-07-17 18:02_

---
