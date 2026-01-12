```yaml
number: 13742
title: Support reading dependency-groups from pyproject.tomls with no project
type: pull_request
state: merged
author: Gankra
labels:
  - bug
assignees: []
merged: true
base: main
head: gankra/group-refactor
created_at: 2025-05-30T20:02:34Z
updated_at: 2025-06-13T22:22:36Z
url: https://github.com/astral-sh/uv/pull/13742
synced_at: 2026-01-12T16:10:50Z
```

# Support reading dependency-groups from pyproject.tomls with no project

---

_@Gankra_

(or legacy tool.uv.workspace).

This cleaves out a dedicated SourcedDependencyGroups type based on RequiresDist but with only the DependencyGroup handling implemented. This allows `uv pip` to read `dependency-groups` from pyproject.tomls that only have that table defined, per PEP 735, and as implemented by `pip`.

However we want our implementation to respect various uv features when they're available:

* `tool.uv.sources`
* `tool.uv.index`
* `tool.uv.dependency-groups.mygroup.requires-python` (#13735)

As such we want to opportunistically detect "as much as possible" while doing as little as possible when things are missing. The issue with the old RequiresDist path was that it fundamentally wanted to build the package, and if `[project]` was missing it would try to desperately run setuptools on the pyproject.toml to try to find metadata and make a hash of things.

At the same time, the old code also put in a lot of effort to try to pretend that `uv pip` dependency-groups worked like `uv` dependency-groups with defaults and non-only semantics, only to separate them back out again. By explicitly separating them out, we confidently get the expected behaviour.

Note that dependency-group support is still included in RequiresDist, as some `uv` paths still use it. It's unclear to me if those paths want this same treatment -- for now I conclude no.

Fixes #13138


---

_@Gankra reviewed on 2025-05-30 20:05_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/dependency_groups.rs`:93 on 2025-05-30 20:05_

This unfortunately merge-conflicts with the FlatDependencyGroups change in #13735 but the fix is trivial (just copy the impl from RequiresDist). 

(This is why I was working on both of these at the same time, I couldn't work out how much they interdepended or would simplify eachother -- in the end they were fairly orthogonal).

---

_@Gankra reviewed on 2025-05-30 20:08_

---

_Review comment by @Gankra on `crates/uv-workspace/src/workspace.rs`:1545 on 2025-05-30 20:08_

This part is why the existing virtual workspace support is insufficient (at least one test fails if I try to do this universally -- although it's a test that specifically checks that we error out if `[project]` and `[tool.uv.workspace]` are missing).

---

_@Gankra reviewed on 2025-05-30 20:09_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:215 on 2025-05-30 20:09_

TODO....

---

_Comment by @Gankra on 2025-05-30 20:10_

As always: this requires Way More Tests...

---

_@Gankra reviewed on 2025-05-30 20:12_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:215 on 2025-05-30 20:12_

I think the assert is telling me I'm *still* using the wrong API though:

> This method requires an absolute path and panics otherwise, i.e. this method only supports discovering the main workspace.

I think this is saying it won't properly detect a package under a workspace?

---

_Label `bug` added by @Gankra on 2025-05-30 20:15_

---

_Comment by @codspeed-hq[bot] on 2025-05-30 20:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/gankra%2Fgroup-refactor)

### Merging #13742 will **not alter performance**

<sub>Comparing <code>gankra/group-refactor</code> (fb63a50) with <code>main</code> (5021840)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Review requested from @charliermarsh by @konstin on 2025-06-02 16:32_

---

_Assigned to @konstin by @zanieb on 2025-06-02 22:26_

---

_Comment by @zanieb on 2025-06-02 22:27_

@konstin I think it's good for Charlie to review, but it'd be good for you to own other support that's needed here.

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-06 16:44_

Why do we need to check this here?

---

_Review comment by @konstin on `crates/uv/src/commands/pip/operations.rs`:13 on 2025-06-06 16:47_

nit: import sorting

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/dependency_groups.rs`:46 on 2025-06-06 16:48_

Can you add a docstring?

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/dependency_groups.rs`:72 on 2025-06-06 18:18_

Should this be included in the workspace discovery caching, could this reparse our entire workspace?

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/dependency_groups.rs`:47 on 2025-06-06 18:20_

Can we simplify the code by reordering and doing an early return on SourceStrategy::Disabled instead, or is there a symmetry reason to keep this structure?

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/dependency_groups.rs`:175 on 2025-06-06 18:41_

Maybe silly question, but what happens if the group exists in the workspace root but not in the pyproject.toml we're currently in?

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:1444 on 2025-06-06 18:44_

Shouldn't we rather forego workspace discovery then and have some kind of `Workspace::for_non_project_pyproject_toml`?

---

_@konstin reviewed on 2025-06-06 18:44_

---

_@Gankra reviewed on 2025-06-10 18:23_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-10 18:23_

This is an eager exit for "no work to do" which now needs to check if there are groups because we don't shove those things in the source_trees anymore.

---

_@Gankra reviewed on 2025-06-10 18:27_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/dependency_groups.rs`:47 on 2025-06-10 18:27_

...huh that's clever yeah

---

_@Gankra reviewed on 2025-06-10 18:36_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/dependency_groups.rs`:175 on 2025-06-10 18:36_

It won't be found.

---

_@Gankra reviewed on 2025-06-10 18:38_

---

_Review comment by @Gankra on `crates/uv-workspace/src/workspace.rs`:1444 on 2025-06-10 18:38_

So the issue is that we *do* want to do workspace discovery *if we can*. Because we want to respect tool.uv sources/indexes (potentially defined in the parent workspace toml) if they are defined, but we also don't want to error if that kind of stuff is missing.

The entire impl is trying to perform graceful degradation.

---

_@Gankra reviewed on 2025-06-10 18:40_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/dependency_groups.rs`:72 on 2025-06-10 18:40_

Say more? (I am passing in the cache, and it can require reparsing the workspace yes.)

---

_@konstin reviewed on 2025-06-11 11:11_

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/dependency_groups.rs`:72 on 2025-06-11 11:11_

ah it already has the right cache

---

_Comment by @Gankra on 2025-06-11 13:29_

(nb this is temporarily based on top of #13735 because the two merge conflict)

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-12 09:25_

my question is more like, don't the groups get folded into the regular requirements in a way that we can fast-path them in `site_packages.satisfies_spec` too?

---

_@konstin reviewed on 2025-06-12 09:25_

---

_@Gankra reviewed on 2025-06-12 13:24_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-12 13:24_

No, they're kept completely separate at this point, they get merged after this check (like, I had to add this && to fix the tests).

---

_@Gankra reviewed on 2025-06-12 13:51_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:215 on 2025-06-12 13:51_

Ok I now properly understand this code and the assert and my solution are both essentially correct, but the scary "this can only discover the root" warning is wrong or misleading. Cleaned it up.

---

_@konstin reviewed on 2025-06-12 14:18_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-12 14:18_

Does that mean we always take the slow path if `--group` is provided, even if we're in a noop case? It doesn't necessarily need to happen in this PR, but I'd want to avoid we're an order of magnitude slower for a noop `pip install --group` than a noop `pip install` without groups.

---

_@Gankra reviewed on 2025-06-12 14:30_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-12 14:30_

Before this PR the source_trees wouldn't be empty and so this check also failed.

---

_@konstin reviewed on 2025-06-12 14:44_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/install.rs`:257 on 2025-06-12 14:44_

I see, let's track this separately: https://github.com/astral-sh/uv/issues/13999

---

_Review comment by @konstin on `crates/uv/src/commands/pip/operations.rs`:251 on 2025-06-12 14:58_

Can we make this an option instead?

---

_Comment by @konstin on 2025-06-13 09:53_

Could you add like a table for the different cases of  the current interactions of workspace and sources when we read dependency groups? E.g., a no-project file that is {member,root} of a workspace where the {root, a member} defines sources. I know these didn't change with this PR, but it would be helpful to have them documented.

---

_Comment by @zanieb on 2025-06-13 13:33_

If they're not changing in this pull request we probably shouldn't block this pull request on it? Can we open an issue for that and follow-up there?

---

_Comment by @konstin on 2025-06-13 13:39_

We don't change the behavior for workspace discovery and sources here, but we apply them to a new class of inputs, and it helps for reviewing to have a clear understanding which semantics we're applying, especially with non-project `pyproject.toml` in workspace being somewhat of an odd case.

---

_Comment by @Gankra on 2025-06-13 14:12_

> E.g., a no-project file that is {member,root} of a workspace where the {root, a member} defines sources.

So I think the crux of "nothing interesting has changed here" is that a no-project file cannot be a member of a workspace. It can only be the root of a workspace. The mode I added just allows you to omit the [tool.uv.workspace] file, in which case it's the only member of its workspace.

---

_Comment by @Gankra on 2025-06-13 14:13_

Furthermore, this ultra-special case I've added is *only* permitted by `uv pip --group`

---

_Comment by @konstin on 2025-06-13 14:16_

> So I think the crux of "nothing interesting has changed here" is that a no-project file cannot be a member of a workspace. It can only be the root of a workspace. 

I see, that explains it!


---

_@konstin approved on 2025-06-13 16:21_

---

_@Gankra reviewed on 2025-06-13 18:00_

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/operations.rs`:251 on 2025-06-13 18:00_

Huh ok this is used way less than I thought, literally only in this one place. Cool fixed.

---

_Merged by @Gankra on 2025-06-13 22:16_

---

_Closed by @Gankra on 2025-06-13 22:16_

---

_Branch deleted on 2025-06-13 22:16_

---
