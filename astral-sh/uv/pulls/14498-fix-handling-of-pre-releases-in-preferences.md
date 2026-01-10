```yaml
number: 14498
title: Fix handling of pre-releases in preferences
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/preference-source
created_at: 2025-07-07T22:02:17Z
updated_at: 2025-07-08T01:10:37Z
url: https://github.com/astral-sh/uv/pull/14498
synced_at: 2026-01-10T06:53:02Z
```

# Fix handling of pre-releases in preferences

---

_Pull request opened by @zanieb on 2025-07-07 22:02_

Closes https://github.com/astral-sh/uv/issues/14485

I tested this using the reproduction in the issue. It'd be nice to add test coverage though.


---

_Renamed from "zb/preference source" to "Fix handling of pre-releases in preferences" by @zanieb on 2025-07-07 22:02_

---

_@zanieb reviewed on 2025-07-07 22:05_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 22:05_

This is the crux of the change. I believe relying on `marker.is_true()` to determine if we should respect a preference was fallible for cases where the preference came an existing fork in the lockfile with markers attatched.

---

_@zanieb reviewed on 2025-07-07 22:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:1733 on 2025-07-07 22:05_

This renamed to disambiguate from `Source`

---

_Review requested from @charliermarsh by @charliermarsh on 2025-07-07 22:06_

---

_Label `bug` added by @charliermarsh on 2025-07-07 22:06_

---

_Review comment by @zanieb on `crates/uv-resolver/src/preferences.rs`:192 on 2025-07-07 22:07_

A little annoyed by the number of source variants, but propagating them from the original location seemed better than assuming one variant during `Preferences::from_iter` and another during `Preferences::insert` (which was my first implementation)

---

_@zanieb reviewed on 2025-07-07 22:07_

---

_@charliermarsh reviewed on 2025-07-07 22:30_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:1733 on 2025-07-07 22:30_

Yeah that's good.

---

_@charliermarsh reviewed on 2025-07-07 22:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 22:32_

I don't think I remember / understand the "rather than a fork" / "comes from a solve without a fork" part, can you help me with that? In other words, why do we have both `fork` and `resolve`?

---

_@zanieb reviewed on 2025-07-07 22:45_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 22:45_

Previously we used `marker.is_true()` to capture the case where `resolution.env.try_universal_markers()` returns `None` (which indicates we're not in a fork) and is unwrapped to `UniversalMarker::TRUE` and propagated here. Now, we just move that up to https://github.com/astral-sh/uv/pull/14498/files#diff-6be6d80fe4821c47b70a372260f55e73b8da8182b8dcad7525d5cd3eb584532bR448-R452.

I don't actually know if we should do something different with `PreferenceSource::Resolve`, but this is intended to match the existing behavior in case it was important — I don't actually know the original motivation either. I could do some digging.

---

_@charliermarsh reviewed on 2025-07-07 23:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 23:17_

It looks like it's been around for a long time: https://github.com/astral-sh/uv/pull/5736. I guess this was a proxy for detecting whether the pre-release came from a lock file? I suspect you could collapse the fork and resolve cases, and make them both false...

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 23:30_

I'm down for trying it.

---

_@zanieb reviewed on 2025-07-07 23:30_

---

_@charliermarsh reviewed on 2025-07-07 23:35_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:301 on 2025-07-07 23:35_

The PR suggests we have scenarios to test this already, so worth trying I think.

---

_@charliermarsh approved on 2025-07-08 01:00_

---

_Marked ready for review by @zanieb on 2025-07-08 01:04_

---

_Merged by @zanieb on 2025-07-08 01:10_

---

_Closed by @zanieb on 2025-07-08 01:10_

---

_Branch deleted on 2025-07-08 01:10_

---
