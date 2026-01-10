```yaml
number: 13635
title: Fix cases where the uv lock is incorrectly marked as out of date
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/normal-groups
created_at: 2025-05-24T00:51:54Z
updated_at: 2025-05-26T18:29:18Z
url: https://github.com/astral-sh/uv/pull/13635
synced_at: 2026-01-10T11:10:42Z
```

# Fix cases where the uv lock is incorrectly marked as out of date

---

_Pull request opened by @Gankra on 2025-05-24 00:51_

PackageMetadata, for whatever reason, does not have a mirrored Wire type so it was easy to not realize that it contains markers that need to be complexified.

Fixes #13614 

---

_Label `bug` added by @Gankra on 2025-05-24 00:51_

---

_@Gankra reviewed on 2025-05-24 00:52_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2858 on 2025-05-24 00:52_

This is technically a drive-by fix; ideally I write a test that shows this was also broken.

---

_@Gankra reviewed on 2025-05-24 00:53_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2841 on 2025-05-24 00:53_

Should we have a PackageMetadataWire type? Is it problematic that we're serializing raw Requirements and not the SimplifiedMarkerTree stuff DependencyWire has?

---

_@Gankra reviewed on 2025-05-24 00:56_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2852 on 2025-05-24 00:56_

All the Results here are actually pointless, but I only realized this at the very end and I'm calling it a night. I should remove them (DependencyWire::unwire is fallible because it resolves packageids, not because it complexifies marker).

---

_@Gankra reviewed on 2025-05-24 00:59_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2858 on 2025-05-24 00:59_

hmm ok actually doing this causes massive breakage...

---

_Comment by @Gankra on 2025-05-24 01:07_

Hmm ok this sure fixes my one test and causes a dozen failures elsewhere... so not quite the right approach 😄 

---

_Comment by @Gankra on 2025-05-24 01:13_

Note to self: Lock::to_toml explicitly Simplifies the other markers, but when it comes to these Requirements it just serializes them directly... Suspicious.

---

_@charliermarsh reviewed on 2025-05-24 12:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:2857 on 2025-05-24 12:17_

Don't we need to `unwire` the `requires_dist` too?

---

_@Gankra reviewed on 2025-05-24 17:33_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:2857 on 2025-05-24 17:33_

I have that in an earlier commit and it caused even more test failures. I imagine yes it wants the same treatment as these dependency-groups, whatever the correct treatment is.

---

_@konstin reviewed on 2025-05-26 11:06_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2857 on 2025-05-26 11:06_

I could confirm this with the case below (failing on the branch), we should solve that one too.

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.11"
dependencies = [
  "pytest; python_full_version>'3.8' and python_full_version<'3.6'"
]
```

---

_@konstin reviewed on 2025-05-26 11:26_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:2857 on 2025-05-26 11:26_

Do the test failures happen because we complexify with requires python? An alternative approach would be filtering out false markers entirely (they can never be used, we don't need to serialize them) or to make the marker parser aware of the `python[_full]_version < 0` hack to restore symmetry between serializing and deserializing. 

---

_Comment by @charliermarsh on 2025-05-26 16:48_

I think the problem here is just in the lockfile satisfies check. We're now comparing simplified to un-simplified markers or something.

---

_Comment by @charliermarsh on 2025-05-26 16:55_

I pushed a change that I think resolves the failures?

---

_@charliermarsh reviewed on 2025-05-26 16:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:4735 on 2025-05-26 16:56_

I'm a little confused as to whether this should be simplify or complexify. It might not matter as long as it's consistent?

---

_@Gankra reviewed on 2025-05-26 17:19_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:4735 on 2025-05-26 17:19_

Yeah that sounds right to me.

---

_Comment by @Gankra on 2025-05-26 17:19_

Added konsti's test

---

_@charliermarsh approved on 2025-05-26 18:14_

---

_Merged by @Gankra on 2025-05-26 18:17_

---

_Closed by @Gankra on 2025-05-26 18:17_

---

_Branch deleted on 2025-05-26 18:17_

---

_Renamed from "Unwire PackageMetadata fields" to "Fix cases where the uv lock is incorrectly marked as out of date" by @Gankra on 2025-05-26 18:29_

---
