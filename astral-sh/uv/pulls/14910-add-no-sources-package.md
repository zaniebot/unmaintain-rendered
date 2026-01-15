```yaml
number: 14910
title: "Add `--no-sources-package`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/no-sources-package
created_at: 2025-07-25T21:11:34Z
updated_at: 2026-01-15T19:53:46Z
url: https://github.com/astral-sh/uv/pull/14910
synced_at: 2026-01-15T20:53:17Z
```

# Add `--no-sources-package`

---

_@zanieb_

I needed this for a test, e.g., to disable a source for an extra build dependency without disabling the source for a workspace member, and had also seen some requests for it. I think it makes sense to allow this.

The refactor is fairly mechanical, we go from `SourceStrategy::Enabled|Disabled` to `NoSources::All|None|Package(names)` as we do for other options like `NoBinary`.

Related https://github.com/astral-sh/uv/issues/17441

---

_Label `cli` added by @zanieb on 2025-07-25 21:11_

---

_Marked ready for review by @zanieb on 2026-01-14 17:50_

---

_Review requested from @charliermarsh by @zanieb on 2026-01-15 03:33_

---

_@charliermarsh reviewed on 2026-01-15 03:35_

---

_Review comment by @charliermarsh on `crates/uv-build-frontend/src/lib.rs`:1044 on 2026-01-15 03:35_

`no_sources.no_sources()` is a bit awkward.

---

_@charliermarsh reviewed on 2026-01-15 03:36_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:219 on 2026-01-15 03:36_

I think I would expect this to return `&NoSources`? Callers can clone if they need to.

---

_@charliermarsh approved on 2026-01-15 03:38_

Looks reasonable. I find `NoSources` name to be a bit confusing because it's sort of like a double-negative? I probably would've left it as `SourceStrategy` and just added a `Disabled(Vec<Package>)` variant.

---

_Comment by @zanieb on 2026-01-15 03:51_

Hm. I was mostly mirroring the `NoBinary` and `NoBuild` patterns. I think it'd need to be `SourceStrategy::Disabled` and `SourceStrategy::DisabledPackages` or `SourceStrategy::Disabled(Option<Vec<Packages>>)`? both of which feel a little weird to me too.

---

_@zanieb reviewed on 2026-01-15 03:58_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:219 on 2026-01-15 03:58_

I think I did this for some painful reason, but it was a while ago now. I'll look again.

---

_Merged by @zanieb on 2026-01-15 19:53_

---

_Closed by @zanieb on 2026-01-15 19:53_

---

_Branch deleted on 2026-01-15 19:53_

---
