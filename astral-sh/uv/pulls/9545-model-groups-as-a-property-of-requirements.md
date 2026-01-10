```yaml
number: 9545
title: Model groups as a property of requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/group
created_at: 2024-12-01T01:38:47Z
updated_at: 2024-12-04T00:55:52Z
url: https://github.com/astral-sh/uv/pull/9545
synced_at: 2026-01-10T12:00:00Z
```

# Model groups as a property of requirements

---

_Pull request opened by @charliermarsh on 2024-12-01 01:38_

## Summary

Today, our dependency group implementation is a little awkward... For each package `P`, we check if `P` contains dependencies for each enabled group, then add a dependency on `P` with the group enabled. There are a few issues here:

1. It's sort of backwards... We add a dependency from the base package `P` to `P` with the group enabled. Then `P` with the group enabled adds a dependency on the base package.
2. We can't, e.g., enable different groups for different packages. (We don't have a way for users to specify this on the CLI, but there's no reason that it should be _impossible_ in the resolver.)
3. It's inconsistent with how extras work, which leads to confusing differences in the resolver.

Instead, our internal requirement type can now include dependency groups, which makes dependency groups look much, much more like extras in the resolver.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-01 01:38_

---

_Review requested from @konstin by @charliermarsh on 2024-12-01 01:38_

---

_Label `internal` added by @charliermarsh on 2024-12-01 01:38_

---

_@charliermarsh reviewed on 2024-12-01 02:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/workspace.rs`:1628 on 2024-12-01 02:27_

\cc @zanieb -- I think you pointed out this general problem long ago.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:89 on 2024-12-03 12:53_

As with #9582, I suspect you'll want to do the same thing for groups as I did for extras: conditionally add the base package dependency if the group is declared as conflicting.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 12:54_

If `requirement.groups` isn't empty at this point, does this mean that all of those groups get skipped?

---

_@BurntSushi reviewed on 2024-12-03 12:54_

---

_@charliermarsh reviewed on 2024-12-03 13:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 13:54_

The assumption is that they’re mutually exclusive

---

_@BurntSushi reviewed on 2024-12-03 13:57_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 13:57_

Interesting. Hard to tell from the type definition. But encoding that invariant in the types is probably annoying.

---

_@charliermarsh reviewed on 2024-12-03 14:23_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 14:23_

I could do it? It's a bit tedious because these also get serialized so we'd want to maintain backwards compatibility, but I could do it. Somewhat prefer to just document it clearly though.

---

_@BurntSushi reviewed on 2024-12-03 14:27_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 14:27_

Documenting it seems fine, yeah.

I am pretty much always torn about how much to encode in the types. I do like it, because I find it makes refactoring without fear a lot easier. It prevents you from getting the case analysis wrong. But as you rightly point out, it's a pain. And if the invariants change, it can make refactoring a pain (albeit higher confidence) too.

It's a constant balancing act.

---

_@zanieb reviewed on 2024-12-03 14:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/workspace.rs`:1628 on 2024-12-03 14:51_

Nice :D

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-03 23:29_

---

_@charliermarsh reviewed on 2024-12-03 23:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:89 on 2024-12-03 23:29_

Done.

---

_@charliermarsh reviewed on 2024-12-03 23:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:39 on 2024-12-03 23:29_

Added.

---

_@BurntSushi approved on 2024-12-04 00:46_

---

_Merged by @charliermarsh on 2024-12-04 00:55_

---

_Closed by @charliermarsh on 2024-12-04 00:55_

---

_Branch deleted on 2024-12-04 00:55_

---
