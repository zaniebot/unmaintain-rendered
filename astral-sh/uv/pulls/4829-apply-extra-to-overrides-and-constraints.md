```yaml
number: 4829
title: Apply extra to overrides and constraints
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/join-markers-for-requirements
created_at: 2024-07-05T09:34:22Z
updated_at: 2024-08-01T07:23:38Z
url: https://github.com/astral-sh/uv/pull/4829
synced_at: 2026-01-10T13:37:23Z
```

# Apply extra to overrides and constraints

---

_Pull request opened by @konstin on 2024-07-05 09:34_

This is an attempt to solve https://github.com/astral-sh/uv/issues/ by applying the extra marker of the requirement to overrides and constraints.

Say in `a` we have a requirements
```
b==1; python_version < "3.10"
c==1; extra == "feature"
```

and overrides
```
b==2; python_version < "3.10"
b==3; python_version >= "3.10"
c==2; python_version < "3.10"
c==3; python_version >= "3.10"
```

Our current strategy is to discard the markers in the original requirements. This means that on 3.12 for `a` we install `b==3`, but it also means that we add `c` to `a` without `a[feature]`, causing #4826. With this PR, the new requirement become,

```
b==2; python_version < "3.10"
b==3; python_version >= "3.10"
c==2; python_version < "3.10" and extra == "feature"
c==3; python_version >= "3.10" and extra == "feature"
```

allowing to override markers while preserving optional dependencies as such.

Fixes #4826

---

_Review requested from @charliermarsh by @konstin on 2024-07-05 09:34_

---

_Comment by @charliermarsh on 2024-07-05 11:58_

I think we do need to join extra markers specifically here...

---

_Comment by @charliermarsh on 2024-07-05 11:59_

Another reasonable option would be to ignore markers in the overrides file and just respect the original markers, but that means you can't replace dependencies on a per-platform basis.

---

_Renamed from "Join requirement and override markers" to "Apply extra to overrides and " by @konstin on 2024-07-08 10:59_

---

_Renamed from "Apply extra to overrides and " to "Apply extra to overrides and constraints" by @konstin on 2024-07-08 10:59_

---

_Marked ready for review by @konstin on 2024-07-08 11:11_

---

_Comment by @konstin on 2024-07-08 11:12_

Changed this to only apply extras, properly ready now.

---

_@charliermarsh reviewed on 2024-07-08 13:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1469 on 2024-07-08 13:10_

Why clone here?

---

_@konstin reviewed on 2024-07-08 16:26_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1469 on 2024-07-08 16:26_

This is now a `Cow`, so we're generally cloning a ref still. Trying to avoid this clone cause the borrow checker to yell a lot because we're using a reference to the requirement for the chained iterator and it doesn't like the closures with a reference to a value we're also moving.

---

_@charliermarsh approved on 2024-07-09 17:54_

---

_Merged by @konstin on 2024-07-09 18:37_

---

_Closed by @konstin on 2024-07-09 18:37_

---

_Branch deleted on 2024-07-09 18:37_

---

_@charliermarsh reviewed on 2024-08-01 00:06_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/constraints.rs`:70 on 2024-08-01 00:06_

@konstin -- This is really random, but is this necessary for constraints? By adding a constraint, we know that the base package was added as a dependency, right?

---

_@charliermarsh reviewed on 2024-08-01 00:07_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/constraints.rs`:70 on 2024-08-01 00:07_

Also, we already merge markers for constraints.

---

_@konstin reviewed on 2024-08-01 07:23_

---

_Review comment by @konstin on `crates/uv-configuration/src/constraints.rs`:70 on 2024-08-01 07:23_

Hmm not sure, i imagine something like:

constraint: `bar <2`

requirement: `root[foo] -> bar`

In that case, we should only add `bar<2` for `root[foo]`, not for `root`.

But it could also be we only need this for overrides.

---
