```yaml
number: 3007
title: "Add `uv-workspace` crate with settings discovery and deserialization"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/workspace
created_at: 2024-04-12T04:20:58Z
updated_at: 2024-04-16T17:56:48Z
url: https://github.com/astral-sh/uv/pull/3007
synced_at: 2026-01-10T14:43:31Z
```

# Add `uv-workspace` crate with settings discovery and deserialization

---

_Pull request opened by @charliermarsh on 2024-04-12 04:20_

## Summary

This PR adds basic struct definitions along with a "workspace" concept for discovering settings. (The "workspace" terminology is used to match Ruff; I did not invent it.)

A few notes:

- We discover any `pyproject.toml` or `uv.toml` file in any parent directory of the current working directory. (We could adjust this to look at the directories of the input files.)
- We don't actually do anything with the configuration yet; but those PRs are large and I want this to be reviewed in isolation.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-15 21:07_

---

_Review requested from @konstin by @charliermarsh on 2024-04-15 21:07_

---

_Label `configuration` added by @charliermarsh on 2024-04-15 21:07_

---

_Marked ready for review by @charliermarsh on 2024-04-15 21:07_

---

_@charliermarsh reviewed on 2024-04-15 21:07_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:35 on 2024-04-15 21:07_

These should arguably be under `pip`. I'm not totally sure though to be honest.

---

_@zanieb reviewed on 2024-04-16 02:35_

---

_Review comment by @zanieb on `crates/uv/src/main.rs`:108 on 2024-04-16 02:35_

If we don't have read permissions for the current directory should we error or just not attempt to read a workspace there?

---

_@zanieb reviewed on 2024-04-16 02:39_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:26 on 2024-04-16 02:39_

Does this mean that we'll fail on _any_ uv invocation if there's an unparsable `pyproject.toml` file in an ancestor directory?

This behavior kind of drives me crazy in Ruff.

---

_@zanieb reviewed on 2024-04-16 02:39_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:26 on 2024-04-16 02:39_

I'd rather log a warning and return `None`. I think failing on an unparsable `uv.toml` makes more sense to me though.

---

_@zanieb reviewed on 2024-04-16 02:40_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:69 on 2024-04-16 02:40_

Aren't a bunch of these very pip-compile output specific? I find it weird to consider these resolver options.

---

_@zanieb reviewed on 2024-04-16 02:41_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:35 on 2024-04-16 02:41_

Do we currently have the same `ResolverOptions` and `InstallerOptions` at the top-level _and_ in `pip`?

---

_@zanieb reviewed on 2024-04-16 02:41_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:83 on 2024-04-16 02:41_

I wonder if we should choose a clearer name for this

---

_@charliermarsh reviewed on 2024-04-16 02:51_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:69 on 2024-04-16 02:51_

Yes, absolutely. Hence: https://github.com/astral-sh/uv/pull/3007#discussion_r1566441095

---

_@charliermarsh reviewed on 2024-04-16 02:51_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:26 on 2024-04-16 02:51_

Yes it does.

---

_@charliermarsh reviewed on 2024-04-16 02:52_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:35 on 2024-04-16 02:52_

No, they're _only_ present at the top-level. Arguably, everything should be put under `pip`. The downside is that it creates a lot of nesting, and we may have options here that _are_ applicable to the non-`pip` interface.

---

_@zanieb reviewed on 2024-04-16 02:56_

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:35 on 2024-04-16 02:56_

Hm but I see

```
pub struct PipOptions {
    ...
    pub resolver: Option<ResolverOptions>,
    pub installer: Option<InstallerOptions>,
```

Anyway... I'd rather put them in `pip` and pull them into the top-level as we know we need them? I think there are only a couple that would make sense.

---

_@charliermarsh reviewed on 2024-04-16 03:59_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:35 on 2024-04-16 03:59_

Maybe I messed that up? Will put them in `pip` and keep them there for now.

---

_Review comment by @konstin on `crates/uv-workspace/src/settings.rs`:22 on 2024-04-16 08:10_

```suggestion
/// `[tool]`
pub(crate) struct Tools {
```

---

_Review comment by @konstin on `crates/uv-workspace/src/settings.rs`:31 on 2024-04-16 08:11_

```suggestion
/// `[tool.uv]`
pub struct Options {
```

---

_Review comment by @konstin on `crates/uv-workspace/src/settings.rs`:15 on 2024-04-16 08:14_

```suggestion
/// A pyproject.toml with an (optional) `[tool.uv]` section
pub(crate) struct PyProjectToml {
```

---

_Review comment by @konstin on `crates/uv/src/main.rs`:108 on 2024-04-16 08:36_

I'm for erroring, i'm assuming we have read and write permissions for the entire repo/project and read permissions for dirs above

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:22 on 2024-04-16 09:42_

Does this mean we only pick the closest file?

The scenario i'm thinking about is one where the work dir is `/home/ferris/foo/bar/` and we have:
* `/home/ferris/uv.toml` or `/home/ferris/.config/uv.toml`: Has some general user-level settings
* `/home/ferris/foo/pyproject.toml`: Defines a workspace of python projects and has some project level settings
* `/home/ferris/foo/bar/pyproject.toml`: A member of that workspace. Does not contain settings (i think?)

In this case, i want to first load user-level setting, then override them with the project level settings (implicitly), then resolve `bar` as part of the `foo` workspace

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:16 on 2024-04-16 09:46_

I'm not sure if i'd call that workspace, i'm thinking about having settings, things that are defined e.g. on the user-level, the project level or the cli level which e.g. define indexes, plus having a workspace, an abstraction over a set of packages that are part of single project. That's the main divergence from ruff, in uv (similar to cargo) you're always in a single workspace and/or a single package; i still have to flesh out how to integrate this into uv.

In cargo, you're technically always in a rust workspace, even if you only define a single crate, that's a single crate workspace. Then there's the concept of [configuration](https://doc.rust-lang.org/cargo/reference/config.html) with hierarchical structure and cli overrides.

---

_@konstin approved on 2024-04-16 09:50_

---

_@charliermarsh reviewed on 2024-04-16 13:26_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:22 on 2024-04-16 13:26_

Yeah we only pick the closest file. What if we do the same as Ruff, and skip a `pyproject.toml` that lacks a `tool.uv` entry?

---

_@charliermarsh reviewed on 2024-04-16 13:26_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:16 on 2024-04-16 13:26_

I can call this `configuration` instead?

---

_@zanieb reviewed on 2024-04-16 13:35_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:22 on 2024-04-16 13:35_

That'd make sense to me.

---

_@zanieb reviewed on 2024-04-16 13:37_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:16 on 2024-04-16 13:37_

I've been intending to call it a "workspace" whether there's one package or many in your project. I don't know if we need to introduce a separate term?

---

_@konstin reviewed on 2024-04-16 13:42_

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:22 on 2024-04-16 13:42_

What do you think about https://doc.rust-lang.org/cargo/reference/config.html#hierarchical-structure ?

---

_@konstin reviewed on 2024-04-16 13:55_

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:16 on 2024-04-16 13:55_

I like `configuration`

---

_@charliermarsh reviewed on 2024-04-16 16:40_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/settings.rs`:83 on 2024-04-16 16:40_

`compile_bytecode`? We could change the CLI flag too (and add an alias).

---

_Review comment by @zanieb on `crates/uv-workspace/src/settings.rs`:83 on 2024-04-16 16:45_

That makes sense to me.

---

_@zanieb reviewed on 2024-04-16 16:45_

---

_@charliermarsh reviewed on 2024-04-16 16:53_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:26 on 2024-04-16 16:53_

Fixed this. We show a user-facing warning on `pyproject.toml` but error on `uv.toml`.

---

_Review requested from @zanieb by @charliermarsh on 2024-04-16 16:54_

---

_Comment by @charliermarsh on 2024-04-16 17:02_

I'm just gonna leave it as "workspace" for now. Let's revisit when we add _actual_ workspace, that's all internal terminology and abstractions anyway.

---

_Merged by @charliermarsh on 2024-04-16 17:56_

---

_Closed by @charliermarsh on 2024-04-16 17:56_

---

_Branch deleted on 2024-04-16 17:56_

---
