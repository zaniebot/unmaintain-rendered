```yaml
number: 14906
title: "Add support for `package`-level conflicts in workspaces"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/package-conflicts
created_at: 2025-07-25T20:03:10Z
updated_at: 2025-08-08T12:45:00Z
url: https://github.com/astral-sh/uv/pull/14906
synced_at: 2026-01-10T06:44:33Z
```

# Add support for `package`-level conflicts in workspaces

---

_Pull request opened by @zanieb on 2025-07-25 20:03_

Revives https://github.com/astral-sh/uv/pull/9130

Previously, we allowed scoping conflicting extras or groups to specific packages, e.g. ,`{ package = "foo", extra = "bar" }` for a conflict in `foo[bar]`. Now, we allow dropping the `extra` or `group` bit and using `{ package = "foo" }` directly which declares a conflict with `foo`'s production dependencies.

This means you can declare conflicts between workspace members, e.g.:

```
[tool.uv]
conflicts = [[{ package = "foo" }, { package = "bar" }]]
```

would not allow `foo` and `bar` to be installed at the same time.

Similarly, a conflict can be declared between a package and a group:

```
[tool.uv]
conflicts = [[{ package = "foo" }, { group = "lint" }]]
```

which would mean, e.g., that `--only-group lint` would be required for the invocation.

As with our existing support for conflicting extras, there are edge-cases here where the resolver will _not_ fail even if there are conflicts that render a particular install target unusable. There's test coverage for some of these. We'll still error at install-time when the conflicting groups are selected. Due to the likelihood of bugs in this feature, I've marked it as a preview feature.

I would not recommend reading the commits as there's some slop from not wanting to rebase Andrew's branch.

---

_Review requested from @charliermarsh by @zanieb on 2025-07-29 17:14_

---

_Review requested from @BurntSushi by @zanieb on 2025-07-29 17:14_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:35 on 2025-07-29 18:10_

IIRC, this is the thing I was most uncertain about @charliermarsh 

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:3063 on 2025-07-29 18:15_

@zanieb These locks LGTM I think. What did you do to make this case work?

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:3165 on 2025-07-29 18:17_

Oh interesting. Could this be added outside of the resolver automatically? Hmm, no, I don't think so. It could only be done in the "direct" case I think? Is there a test for the indirect/transitive case? I looked below but I don't think I see this work-around used anywhere else.

---

_@BurntSushi approved on 2025-07-29 18:20_

Nice, I think this LGTM. I think it would still be good to get @charliermarsh's review on the parent package stuff I added (I left a comment noting where).

I was curious what else you did here to make the remaining cases work?

---

_@zanieb reviewed on 2025-07-29 18:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3165 on 2025-07-29 18:28_

I looked into that a bit but decided to cut scope. It seems quite plausible to update the resolver to handle this case when forking.

---

_@zanieb reviewed on 2025-07-29 18:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3063 on 2025-07-29 18:31_

To get `detect_conflicts` to pass, I needed to add 

https://github.com/astral-sh/uv/blob/1dc78400bc4c08f32c8110c29cb515dc656d2108/crates/uv/src/commands/project/mod.rs#L2513-L2516

which required, more broadly, understanding the subset of package we were reasoning about during conflict detection.

Then there were some minor problems in the forking logic. The main thing was 

https://github.com/astral-sh/uv/blob/1dc78400bc4c08f32c8110c29cb515dc656d2108/crates/uv-resolver/src/resolver/mod.rs#L3792-L3798

---

_@zanieb reviewed on 2025-07-29 18:35_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3165 on 2025-07-29 18:35_

There is `lock_conflicting_workspace_members_depends_transitive_extra` but in that case there's a conflict that should not be resolved. I didn't add a transitive case like this one — I felt like I was going to end up playing whackamole with a bunch of complicated test cases. I think if it wasn't in preview, I wouldn't be comfortable without doing a lot more testing.

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/preview.rs`:17 on 2025-08-01 02:02_

What's the thinking behind releasing this under a flag?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:30 on 2025-08-01 02:03_

"with deal with project level conflicts"

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3800 on 2025-08-01 02:05_

Nit: use a `matches!` since there's only one active branch here?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/install_target.rs`:381 on 2025-08-01 02:06_

I didn't look at the usage thoroughly, but this could probably be `BTreeSet<&PackageName>`, with the same lifetime as `&self`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:35 on 2025-08-01 02:15_

I'm also not 100% sure on the implications here...

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:177 on 2025-08-01 02:16_

How would this branch ever get hit? Doesn't `self.package.conflicting_item()` return `Some` in all branches now (and `unreachable!`) in the last branch?

---

_@charliermarsh reviewed on 2025-08-01 02:16_

---

_@zanieb reviewed on 2025-08-02 12:23_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:3800 on 2025-08-02 12:23_

I wanted it to be exhaustive so it needs to be revisited if we add another kind, but I don't feel strongly.

---

_@zanieb reviewed on 2025-08-02 12:24_

---

_Review comment by @zanieb on `crates/uv-configuration/src/preview.rs`:17 on 2025-08-02 12:24_

It's in the summary

> As with our existing support for conflicting extras, there are edge-cases here where the resolver will not fail even if there are conflicts that render a particular install target unusable. There's test coverage for some of these. We'll still error at install-time when the conflicting groups are selected. Due to the likelihood of bugs in this feature, I've marked it as a preview feature.

I'm just not sure we'll be able to meet our bar for correctness here in the first iteration.

---

_@zanieb reviewed on 2025-08-02 12:52_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/dependencies.rs`:177 on 2025-08-02 12:52_

It _can_ return `None`

https://github.com/astral-sh/uv/blob/8afbd86f03b21cd8a77663da166b66aa0be62acf/crates/uv-resolver/src/pubgrub/package.rs#L222

I'm not sure if it's needed. I'll poke around.

---

_@zanieb reviewed on 2025-08-04 15:54_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/dependencies.rs`:177 on 2025-08-04 15:54_

I decided to remove it, since it has no effect and I don't see a reason for it. It does seem reachable though.

---

_@charliermarsh approved on 2025-08-04 17:35_

---

_Label `preview` added by @zanieb on 2025-08-07 13:57_

---

_Renamed from "Add support for `package`-level conflicts" to "Add support for `package`-level conflicts in workspaces" by @zanieb on 2025-08-07 13:57_

---

_Merged by @zanieb on 2025-08-08 12:44_

---

_Closed by @zanieb on 2025-08-08 12:44_

---

_Branch deleted on 2025-08-08 12:45_

---
