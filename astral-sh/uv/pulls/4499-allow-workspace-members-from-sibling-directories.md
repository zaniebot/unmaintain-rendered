```yaml
number: 4499
title: Allow workspace members from sibling directories
type: pull_request
state: closed
author: idlsoft
labels: []
assignees: []
base: main
head: sibling_ws_members
created_at: 2024-06-25T00:17:29Z
updated_at: 2024-07-18T20:49:37Z
url: https://github.com/astral-sh/uv/pull/4499
synced_at: 2026-01-10T13:42:52Z
```

# Allow workspace members from sibling directories

---

_Pull request opened by @idlsoft on 2024-06-25 00:17_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Shared libraries for independent projects. 

Let's say I have the following layout:
```
src_root
├── app1
├── app2
├── shared_lib
└── shared_corelib
```

`app1` and `app2` never have to coexist in the same virtualenv, their dependencies are independent
and allowed to conflict with each other. `shared_lib` and `shared_corelib`, being libraries, have to play nice with both of them.

I want to use worskpaces to define the following direct dependencies:
 - `app1->shared_lib`
 - `app2->shared_lib`
 -  `shared_lib->shared_corelib`

But if I simply create a workspace in `src_root` it'll try to create a shared lock and shared virtualenv for all four projects.
And if I create a workspace in either `app1` or `app2`, they won't let me include `shared_lib` because it's in a sibling directory, and workspaces require a single root.

At my previous job I've implemented a build, that assumed this kind of scenario, and the workspace part was only responsible for locating members, and storing shared settings like default dependency versions, constraints, etc. This has the downside of having many virtualenvs so may not be for everyone, but I think it's a valid case.

I don't know if there are any future plans to support this in `uv`, perhaps by introducing the notion of a "leaf" member?

In the meantime, the scenario could be supported if workspaces were allowed to include sibling directories.
This is what this PR does.

## Test Plan

<!-- How was it tested? -->
Unit test

---

_Review requested from @konstin by @zanieb on 2024-06-25 01:21_

---

_Marked ready for review by @idlsoft on 2024-06-25 03:43_

---

_Converted to draft by @idlsoft on 2024-06-25 04:23_

---

_Marked ready for review by @idlsoft on 2024-06-25 16:59_

---

_Comment by @konstin on 2024-06-25 18:26_

The reason we've been enforcing that package need to be below the workspace root is for dependencies into other workspace. Say we have a workspace:
```
A
|- B
|- C
```
and another
```
D
|- E
|- F
```
and dependencies B -> E and E -> F. With our current design, E can say `F = { workspace = true }` and we can figure out the correct paths for B -> E -> F by walking up the tree. This feature is also supported by cargo.

For your use case, can you use a regular editable path dependency instead of a workspace?

---

_Comment by @idlsoft on 2024-06-25 19:54_

> and dependencies B -> E and E -> F. With our current design, E can say `F = { workspace = true }` and we can figure out the correct paths for B -> E -> F by walking up the tree. This feature is also supported by cargo.

In this example `B` would have to make sure that both `E` and `F` are included in its workspace definition.
cargo is a bit different, because it doesn't have to create a shared virtualenv, and independent crates are allowed to have conflicting dependencies. You also can't reference a library you don't depend on, just because it happens to be in the same workspace.

I know this is not the most elegant solution.
Here is, I think, a nicer one:

There is a method in [`workspace.rs`](https://github.com/astral-sh/uv/blob/af1f1369e51191692ccace224393ad6fd20685aa/crates/uv-distribution/src/workspace.rs#L147):
```rust
pub fn with_current_project(self, package_name: PackageName) -> Option<ProjectWorkspace> 
```
It could read the `pyproject.toml` of `package_name` and look for something like
```toml
[tool.uv]
top-level = true
```
If that's set it would filter out members that aren't a direct or transitive dependency of `package_name`, and make `package_name` the workspace root, thus driving the location of `uv.lock`, `.venv`, overrides etc.


> For your use case, can you use a regular editable path dependency instead of a workspace?

I tried that a while ago, but `uv` and `hatch` were fighting each other, and the only way I got it to work was with [this](https://github.com/astral-sh/uv/pull/3949), another bandaid.

I'm not a big fan of either of these PRs, ideally I'd like to see workspaces support independent members natively.
If what I described above doesn't conflict with any ongoing work or future plans, I can try implementing it.


---

_Comment by @idlsoft on 2024-06-26 21:48_

> In this example `B` would have to make sure that both `E` and `F` are included in its workspace definition.

After looking at the source a bit more I realize this isn't quite right.
Projects need to be able to lookup for their workspaces.

I updated the sample workspace to allow dependencies between libraries.


---

_Comment by @konstin on 2024-06-27 08:05_

I am aware of the current problems when trying to combine relative paths, PEP 621/pyproject.toml and a build backend, but i don't think workspaces are the right tool to solve. Workspaces are intended for cases where there's a single project consisting of multiple package, to give each package an individual dependency specification and - if wanted - configuration in pyproject.toml. They don't work well as a tool for sharing a library between two applications.

---

_Comment by @idlsoft on 2024-06-27 10:25_

> They don't work well as a tool for sharing a library between two applications.

That's true, but I think with a few tweaks they could provide a clean solution for both.

This particular change is a few lines only, doesn't break any existing code, and already makes the scenario possible.

I also opened  #4574, proposing a friendlier solution. It'll require a bit more coding, but I don't think it's going to be too intrusive either. 

---

_Comment by @idlsoft on 2024-06-28 13:51_

The [new PR](https://github.com/astral-sh/uv/pull/4610) provides explicit support for the use-case.
It's a lot more straightforward from an end-user perspective, and interestingly didn't require the change from `.strip_prefix` to `relative_to`.

---

_Comment by @konstin on 2024-06-28 14:30_

Currently it doesn't break any existing code, but with https://github.com/astral-sh/uv/issues/3943 in place and people starting to have e.g. git dependencies into a package in a workspace, i'm afraid this change would cause some breakage.

If you could write-up an issue about your problems using hatch and uv together with relative path dependencies, i can have look and we can figure out a way to make this work. Starting off with a specific problem and discussing possible solutions and existing constraints we can much better solve your use case and avoid rejecting PRs that you've put effort into.

---

_Comment by @idlsoft on 2024-06-28 15:40_

> with #3943 in place and people starting to have e.g. git dependencies into a package in a workspace, i'm afraid this change would cause some breakage.

I believe this PR kinda close the 1st bullet point from #3943, while https://github.com/astral-sh/uv/pull/4610 addresses the same underlying issue differently, within a single workspace, with no extra toml to write.

> If you could write-up an issue about your problems using hatch and uv together with relative path dependencies, i can have look and we can figure out a way to make this work.

I'll try to come up with an example. But from what I remember, it was something like
* `uv` doesn't allow `lib @ ../lib` and didn't like `{root:uri}`, but supports `${PROJECT_ROOT}`
* `hatch` doesn't like `${PROJECT_ROOT}` but is happy with `lib @ ../lib` and its own `{root:uri}`

> Starting off with a specific problem and discussing possible solutions and existing constraints we can much better solve your use case and avoid rejecting PRs that you've put effort into.

I don't mean to overwhelm you with PRs, but figuring out a solution is fun regardless of whether it gets merged or not.

The problem I'm trying to solve is described in https://github.com/astral-sh/uv/issues/4574.
Multiple apps, using multiple libraries directly, as editable dependencies, coexisting even if their dependencies aren't compatible.
I believe workspaces can provide an elegant way of solving it, and it'll look familiar to anyone who's ever used cargo, or gradle.

---

_Comment by @idlsoft on 2024-07-18 20:49_

The use-case is supported using
```toml
[tool.uv.sources]
shared_lib = { path = "../shared_lib", editable = true}
```

---

_Closed by @idlsoft on 2024-07-18 20:49_

---
