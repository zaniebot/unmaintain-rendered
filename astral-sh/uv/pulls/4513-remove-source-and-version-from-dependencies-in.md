```yaml
number: 4513
title: "remove `source` and `version` from dependencies in lock file when unambiguous"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/lock-dependency-trim
created_at: 2024-06-25T13:01:09Z
updated_at: 2024-06-26T12:18:24Z
url: https://github.com/astral-sh/uv/pull/4513
synced_at: 2026-01-10T13:48:28Z
```

# remove `source` and `version` from dependencies in lock file when unambiguous

---

_Pull request opened by @BurntSushi on 2024-06-25 13:01_

When there is only one distribution for a particular package name, any
dependencies (the edges in the resolution graph) that reference that
package name are completely unambiguous. Therefore, we can actually
omit their version and source information and instead derive it from
the distribution entry.

We add some tests to check the success and error cases. That is,
when `source` or `version` are omitted and there are more than one
corresponding distribution for the package name (i.e., it's ambiguous),
then lock deserialization should fail.

Since there can be a lot of `distribution.dependency` blocks, this
change trims a lot of fat from a lock file.

Cargo does a similar thing here where it omits the source and version
from dependencies when their value is unambiguous.

Ref https://github.com/astral-sh/uv/issues/3611

---

_Review requested from @konstin by @BurntSushi on 2024-06-25 13:07_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-06-25 13:07_

---

_Comment by @charliermarsh on 2024-06-25 15:16_

Sweet!

---

_@ibraheemdev approved on 2024-06-25 20:22_

Nice! This cleans up the lockfile quite a bit.

---

_@charliermarsh approved on 2024-06-26 00:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:251 on 2024-06-26 10:32_

```suggestion
            for dependencies in &mut dist.optional_dependencies.values() {
```

The keys are unused in these iterations

---

_@konstin approved on 2024-06-26 11:12_

---

_Merged by @BurntSushi on 2024-06-26 12:18_

---

_Closed by @BurntSushi on 2024-06-26 12:18_

---

_Branch deleted on 2024-06-26 12:18_

---
