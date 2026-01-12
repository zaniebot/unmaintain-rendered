```yaml
number: 7143
title: "Remove `{ workspace = true }` requirement for sources"
type: issue
state: open
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2024-09-06T22:17:08Z
updated_at: 2025-12-20T01:31:03Z
url: https://github.com/astral-sh/uv/issues/7143
synced_at: 2026-01-12T15:59:10Z
```

# Remove `{ workspace = true }` requirement for sources

---

_@charliermarsh_

We error if a package is in the workspace but isn't marked as `{ workspace = true }`. Should we just infer this? As-is, you need to specify the members up to three times (once in members, once in dependencies, and once in sources).


---

_Label `needs-decision` added by @charliermarsh on 2024-09-06 22:17_

---

_Comment by @charliermarsh on 2024-09-06 22:18_

I think this _probably_ makes sense. The downside is, if this is omitted by default, then we lose the behavior that we error if you mark `workspace = true` but the package _isn't_ found.

---

_Comment by @mmerickel on 2024-09-06 22:35_

I could be missing some nuance but I don't really see the logic in the current behavior. If I add the packages to the workspace then it'd be quite surprising to me for them to not be found/installed from the workspace when other packages depend on them. So to me it's only an improvement to add them to the list of sources by default to avoid having to duplicate them. It's even worse because the original workspace declaration is a glob for me hiding the details, and then I need to explicitly list the packages anyway.

```toml
[project]
# this can technically contain a subset of the packages so it makes sense to declare them here as default dependencies of the workspace
dependencies = ["my-foo", "my-bar", "..."]

# it'd be nice to get rid of this
[tool.uv.sources]
my-foo = { workspace = true }
my-bar = {workspace = true }
# ...

[tool.uv.workspace]
members = ["src/*"]
```

---

_Comment by @mmerickel on 2024-09-06 22:48_

I should also point out that rye's workspaces don't have this extra sources requirement. In rye the packages are automatically workspace-packages when added to `tool.rye.workspace.members`.

---

_Comment by @charliermarsh on 2024-09-06 23:08_

Just to be explicit: if a package is defined in the workspace but not declared as a source, we error rather than proceeding. So we're just being strict in asking users to specify the source explicitly. It might be more annoying than helpful though.

---

_Comment by @charliermarsh on 2024-09-06 23:14_

I think I'm fine to remove this. If a user does it explicitly, we can then error if the package isn't found in the workspace but is marked as `{ workspace = true }`. I guess this is breaking, though... any applications that would be affected by it would _already_ error today, so maybe not.

\cc @konstin @zanieb 

---

_Comment by @konstin on 2024-09-07 18:03_

This behavior was carried over from cargo, which intentionally requires `{ workspace = true }`.  In cargo, with `workspace = true` and `registry = "..."` you can always tell just from `Cargo.toml` where a dependency comes from, you have to explicitly annotate if you want a dependency that's not from the default registry.

I don't expect this change to be breaking, iirc `workspace = true` is a read-only check that doesn't actually change dependency lowering, you'd just have to remote this check:

https://github.com/astral-sh/uv/blob/56cc0c9b3cac860b072d2b22556d441d602a0eb0/crates/uv-distribution/src/metadata/lowering.rs#L47-L63

---

_Comment by @charliermarsh on 2024-09-07 18:09_

@konstin -- Any opinion on changing it?

---

_Comment by @konstin on 2024-09-07 19:23_

I like the explicitness in cargo but I see the awkwardness when applied in a split-table dependencies vs. sources scenario. 

---

_Comment by @Tremeschin on 2024-10-23 15:35_

Having implicit `workspace=true` in sources allows for having private packages/insiders repositories part of the venv glob listed in `members`. I've already migrated to `uv` and failed to notice this beforehand, not breaking for me but not ideal.

Somewhat related, but I find it weird having to re-define the workspace packages in `dev-dependencies` (without a version) for any `[project.scripts]` defined in member projects to create their entry points in the `venv` bin directory.

Maybe I'm doing something wrong, most documentation points to `uv run {cmd}` which I'd only recommend to beginners as it downloads python, creates venv and all, and personally prefer just typing `{cmd}` to run stuff;

point is, listing a package in a rye workspace implies all of that, I had to lurk to get it working 🙂

---

_Renamed from "Remove `{ workspace = true}` requirement for sources" to "Remove `{ workspace = true }` requirement for sources" by @charliermarsh on 2024-10-30 20:22_

---

_Comment by @tsudol-plaid on 2025-12-20 01:31_

An additional point: this would be nice to have because the [workspace docs](https://docs.astral.sh/uv/concepts/projects/workspaces/) recommend using a glob for workspace members. What's the point of accepting a glob for `tool.uv.workspace.members` when you have to go define `tool.uv.sources` for each one anyway?

```toml
[tool.uv.sources]
bird-feeder = { workspace = true }

[tool.uv.workspace]
members = ["packages/*"]
exclude = ["packages/seeds"]
```

But I'll admit that cargo requiring this is a strong argument in its favor.

---
