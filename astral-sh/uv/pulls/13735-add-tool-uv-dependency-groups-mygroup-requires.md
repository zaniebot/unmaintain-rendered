```yaml
number: 13735
title: "Add `[tool.uv.dependency-groups].mygroup.requires-python`"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gankra/group-python-real
created_at: 2025-05-30T17:15:32Z
updated_at: 2025-06-13T22:10:23Z
url: https://github.com/astral-sh/uv/pull/13735
synced_at: 2026-01-10T11:10:42Z
```

# Add `[tool.uv.dependency-groups].mygroup.requires-python`

---

_Pull request opened by @Gankra on 2025-05-30 17:15_

This requires some more tests and almost certainly some error messages, but, I think this is the implementation?

The first commit is a large driveby move of the RequiresPython type to uv-distribution-types to allow us to generate the appropriate markers at this point in the code. It also migrates RequiresPython from pubgrub::Range to version_ranges::Ranges, and makes several pub(crate) items pub, as it's no longer defined in uv_resolver.

Fixes #11606

---

_@Gankra reviewed on 2025-05-30 17:17_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/requires_dist.rs`:145 on 2025-05-30 17:17_

In earlier commits this is done in LowerRequirement::from_requirement as charlie suggested, but after implementing+testing I realized this is "too late" because of dependency-group `includes`, which gets erased during flattening, making us forget where requirements come from.

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/requires_dist.rs`:139 on 2025-05-30 17:19_

Note we source these independent of SourceStrategy -- this is... I think Fine because it's similar to dev-dependencies handling, which is also unconditional. (We litigate this a bit in the past and were leaning towards the behaviour I implement here).

---

_@Gankra reviewed on 2025-05-30 17:19_

---

_@Gankra reviewed on 2025-05-30 17:23_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/requires_dist.rs`:149 on 2025-05-30 17:23_

Note: the legacy special `tool.uv.dev-dependencies` group gets injected post-flattening. Currently this means we don't support `tool.uv.dependency-groups.dev.requires-python` affecting it. I think this is *fine* (you can just migrate to `dependency-groups.dev` if you want to apply requires-python to it).

---

_@Gankra reviewed on 2025-05-30 17:24_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:188 on 2025-05-30 17:24_

This is the actual place where the work happens

---

_@Gankra reviewed on 2025-05-30 17:26_

---

_Review comment by @Gankra on `crates/uv-workspace/src/pyproject.rs`:363 on 2025-05-30 17:26_

```suggestion
        example = r#"
            [tool.uv.dependency-groups]
            mygroup = {requires-python = ">=3.12"}
        "#
```

Is probably more idiomatic here, in terms of the normal dependency-groups table.

---

_@Gankra reviewed on 2025-05-30 17:28_

---

_Review comment by @Gankra on `crates/uv/tests/it/lock.rs`:21014 on 2025-05-30 17:28_

Interesting case where requires-python markers + includes lead to non-obvious contradiction markers (here python >= 3.13 + python < 3.13 result from including one group into another and applying both their requires-pythons as appropriate).

---

_@Gankra reviewed on 2025-05-30 17:30_

---

_Review comment by @Gankra on `crates/uv/tests/it/lock.rs`:20883 on 2025-05-30 17:30_

`>= 3.12.1` becoming `>= 3.12.[X]` is hopefully a Me Learning Stuff moment and not a The Implementation Is Broken moment.

---

_Label `enhancement` added by @Gankra on 2025-05-30 20:15_

---

_Comment by @konstin on 2025-06-02 09:05_

I find the current behavior around adding markers to the group members unintuitive:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.9"
dependencies = ["typing-extensions"]

[dependency-groups]
docs = ["sphinx"]

[tool.uv.dependency-groups]
docs = { requires-python = ">=3.12" }
```

When I now run

```
uv run -p 3.11 --group docs sphinx-build
```

it fails to run sphinx, but it doesn't tell me that the docs groups requires a newer Python version. I would have expected this call to fail and report that I need at least Python 3.12. Similar to how the project `requires-python` works:

```
$ uv run -p 3.8 --group docs sphinx-build
Using CPython 3.8.18 interpreter at: /home/konsti/.local/bin/python3.8
error: The requested interpreter resolved to Python 3.8.18, which is incompatible with the project's Python requirement: `>=3.9`
```

---

_Comment by @Gankra on 2025-06-02 16:35_

I agree I'd prefer a better diagnostic, zanie mentioned last week that I might need to do some work on... i think they said "resolver hints" or something? Or are you saying lowering to markers is entirely wrong, and I need to be resolving this metadata in a different part of uv?

---

_Comment by @konstin on 2025-06-02 16:44_

My preference is that the requires-python is a mandatory bound, so `uv run -p 3.8 --group docs sphinx-build` would error. This would match the name `requires-python` and how `project.requires-python` works (both for the current project and transitively). If we're implementing the current mechanism of adding this as markers to each requirement, I would choose a different name for it to make the semantic differences with `requires-python` clearer.

Separately, do want to land baf989515e8b2b5f38ca7cf9066a87830da430c3 as a split out PR? r+ me it's a pure refactoring anyway.

---

_Comment by @Gankra on 2025-06-02 16:46_

> Separately, do want to land https://github.com/astral-sh/uv/commit/baf989515e8b2b5f38ca7cf9066a87830da430c3 as a split out PR? r+ me it's a pure refactoring anyway.

The ranges migration is good but if moving it out of the crate is based on me misunderstanding this problem I'd rather not land that part.

---

_Comment by @Gankra on 2025-06-02 16:49_

I'll take a closer look at how the top-level requires-python is implemented to better understand what that impl would look like.

---

_Comment by @zanieb on 2025-06-02 16:58_

Konsti is talking about confirming the selected Python interpreter meets the resolved `requires-python` range for the group. You'll need to ensure we're taking the fully resolved `requires-python` request into account when selecting a Python interpreter.

Separately, what I was talking about, is if there are conflicts between the `requires-python` in multiple groups or with the `project.requires-python`.

---

_Comment by @konstin on 2025-06-02 17:10_

My preference is pretty much https://github.com/astral-sh/uv/issues/11606#issuecomment-2735124600: I think the current changes are required to get the right resolver behavior (split at the reuires-python version and resolve the dependencies only in the upper split, since e.g. sphinx has a higher requires-python itself and otherwise fails to resolve; Adding the markers should facilitate that), but I think we should also error when the Python version is too low for the group. CC @zanieb wdyt?

---

_Comment by @zanieb on 2025-06-02 18:28_

Yes, that's correct.

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4658 on 2025-06-10 13:29_

Participating in interpreter discovery

---

_@Gankra reviewed on 2025-06-10 13:29_

---

_@Gankra reviewed on 2025-06-10 13:32_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4727 on 2025-06-10 13:32_

We now get a proper high-level error, although this already-kinda-obtuse error message is now even more cryptic because at this point in the code we've forgotten all the requires-python sources and discarded the enabled groups, so we're just like "here's the intersection of all active requirements in your workspace".

Having better diagnostics about this situation I think should be future work.

---

_@Gankra reviewed on 2025-06-10 13:32_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4740 on 2025-06-10 13:32_

Another pre-existing cryptic error that is now even spookier with group-requires-python.

---

_@Gankra reviewed on 2025-06-10 13:34_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4839 on 2025-06-10 13:34_

In this pre-existing error we *are* still holding onto the sources of all the requires-pythons and dump every active one. I've enhanced it to display group requires-pythons, as can be seen here.

---

_@zanieb reviewed on 2025-06-10 13:36_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4740 on 2025-06-10 13:36_

https://github.com/astral-sh/uv/pull/11964 may help

---

_@Gankra reviewed on 2025-06-10 13:37_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:8949 on 2025-06-10 13:37_

A bunch of malformed dependency-group errors have churned slightly, because group flattenings are now evaluated extremely early (before we've even done interpreter discovery), making it a sort of global error instead of one associated with a particular build.

I think I can add context back here though, the code that hits this still knows what project/pyproject had the issue.

---

_Assigned to @zanieb by @zanieb on 2025-06-10 16:52_

---

_Comment by @codspeed-hq[bot] on 2025-06-10 17:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/gankra%2Fgroup-python-real)

### Merging #13735 will **not alter performance**

<sub>Comparing <code>gankra/group-python-real</code> (403adea) with <code>main</code> (26db29c)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Comment by @Gankra on 2025-06-10 18:20_

(This PR should now be complete and ready to merge, pending approval)

---

_@zanieb reviewed on 2025-06-10 18:22_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dependency_groups.rs`:307 on 2025-06-10 18:22_

Nit: Newline?

---

_@zanieb reviewed on 2025-06-10 18:23_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:36 on 2025-06-10 18:23_

Confusing comment

---

_@zanieb reviewed on 2025-06-10 18:24_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:71 on 2025-06-10 18:24_

```suggestion
        // Add the `dev` group, if the legacy `dev-dependencies` is defined.
```

---

_@zanieb reviewed on 2025-06-10 18:25_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:171 on 2025-06-10 18:25_

It can't be `unwrap_or_default` because it's borrowed?

---

_@zanieb reviewed on 2025-06-10 18:26_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:149 on 2025-06-10 18:26_

What's this clause? A comment may be helpful here.

---

_@zanieb reviewed on 2025-06-10 18:30_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:178 on 2025-06-10 18:30_

I might say...
```suggestion
                // Add the group requires-python as a marker to each requirement
                // We don't use `full_requires_python` because each `include-group`
                // should already have markers.
```

btw what if the `include-group` doesn't have a `requires-python`? Won't the requirements be missing the marker then?

---

_@zanieb reviewed on 2025-06-10 18:31_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:167 on 2025-06-10 18:31_

Is this like..
```suggestion
                // Update the full requires-python to the intersection with this group's requires-python
```

---

_@zanieb reviewed on 2025-06-10 18:32_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:168 on 2025-06-10 18:32_

This is a bit misleading, these are specifiers, not versions, right?

---

_@zanieb reviewed on 2025-06-10 18:33_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:173 on 2025-06-10 18:33_

Should there be a method on `VersionSpecifiers` to do this extension? It's a bit clunky doing it here twice?

---

_@zanieb reviewed on 2025-06-10 18:35_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:123 on 2025-06-10 18:35_

We might want to rename this, since `python_full_version` is a marker. maybe like `requires_python_intersection`?

---

_@Gankra reviewed on 2025-06-10 18:53_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:149 on 2025-06-10 18:53_

This is me getting way too cute and trying to keep the requires_python as an `Option<VersionSpecifiers>`, without a ton of matching on Options -- if both entries are None then we make an empty `Box<[]>`, and then we turn that back into `None` with this `then_some`.

---

_@Gankra reviewed on 2025-06-10 18:53_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:168 on 2025-06-10 18:53_

Yes good call

---

_@Gankra reviewed on 2025-06-10 18:54_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:173 on 2025-06-10 18:54_

Yeah I spent way too long assuming there would be, I'll add it.

---

_@Gankra reviewed on 2025-06-10 18:54_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:171 on 2025-06-10 18:54_

Yep

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:36 on 2025-06-10 19:08_

Refactor gunk, good catch.

---

_@Gankra reviewed on 2025-06-10 19:08_

---

_@Gankra reviewed on 2025-06-10 19:47_

---

_Review comment by @Gankra on `crates/uv-workspace/src/dependency_groups.rs`:178 on 2025-06-10 19:47_

Say we have:

```
[dependency-groups]
dev = ["devdep", {include-group = "test"}]
test = ["testdep"]

[tool.uv.dependency-groups]
dev = {requires-python = ">= 3.10"}
test = {requires-python = "< 3.12"}
```

Then we will first process test and get this, note that the requires-python has been applied to all of its requirements already:

```rust
"test": FlatDependencyGroup {
  requires_python: Some("< 3.12"),
  requirements: ["testdep; python_full_version < 3.12"]
}
```

Then we will process "dev", and we will get this:

```rust
"test": FlatDependencyGroup {
  requires_python: Some(">= 3.10, <3.12"),
  requirements: [
    "testdep; python_full_version >= 3.10, < 3.12",
    "devdep; python_full_version >= 3.10",
  ]
}
```

Note that `testdep`, has both markers on it, while `devdep` only has dev's. 

This is because test's marker was applied when it was computed, and then when computing dev we copied the entire requirement from dev. Then we applied dev's marker to everything that got flattened into it.

If dev had no requires-python then this would produce the correct result: testdep would have a marker and devdep wouldn't.

If test had no requires-python then this would produce the correct result: testdep and devdep would have devdep's marker.

In theory this could permit like "half" a dependency-group to get installed based on your python version, but in practice the top-level interpreter checks will complain because the requires_python_intersection drops that granularity when we hand it off to interpreter discovery/validation.
    

---

_Comment by @Gankra on 2025-06-10 19:51_

(the current test failures are an issue that's on main)

---

_@zanieb reviewed on 2025-06-11 20:33_

---

_Review comment by @zanieb on `crates/uv-workspace/src/pyproject.rs`:365 on 2025-06-11 20:33_

We may need to document that you can't declare group members here and that those need to go in `[dependency-groups]`?

---

_@Gankra reviewed on 2025-06-11 20:38_

---

_Review comment by @Gankra on `crates/uv-workspace/src/pyproject.rs`:365 on 2025-06-11 20:38_

Hmm interesting yeah.

---

_@zanieb reviewed on 2025-06-11 20:47_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:20883 on 2025-06-11 20:47_

We insert filters for the patch version of the Python versions in the environment. It applies to 3.12 because of `let context = TestContext::new("3.12");`

---

_@zanieb reviewed on 2025-06-11 20:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4727 on 2025-06-11 20:50_

Huh? Why is there a reference to a workspace at all here? This doesn't seem quite right, and seems bad enough that we need to improve it here because we're recommending installing a workspace member?

---

_@zanieb reviewed on 2025-06-11 20:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4730 on 2025-06-11 20:50_

Use `with_filtered_python_sources` instead of toggling cfg

---

_@zanieb reviewed on 2025-06-11 20:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4836 on 2025-06-11 20:53_

Are the backticks doing anything for us here?

---

_@zanieb reviewed on 2025-06-11 20:54_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4837 on 2025-06-11 20:54_

In `uv tree`, we'd say

> anyio v4.3.0 (group: foo)

Maybe we should match that format here?

> project (group: bar)

We also have a format for displaying group names in resolver errors, I can't find an example off the top of my head, but we should find an example and consider using it.

---

_@Gankra reviewed on 2025-06-11 20:56_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4836 on 2025-06-11 20:56_

This is a pre-existing format but happy to clean it up

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4837 on 2025-06-11 20:56_

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example/
❯ uv add --group foo ruff==0.2.0
Using CPython 3.13.2
Creating virtual environment at: .venv
Resolved 2 packages in 3ms
Installed 1 package in 2ms
 + ruff==0.2.0
❯ uv add --group bar ruff==0.3.0
  × No solution found when resolving dependencies:
  ╰─▶ Because example:foo depends on ruff==0.2.0 and example:bar depends on ruff==0.3.0, we can conclude that
      example:bar and example:foo are incompatible.
      And because your project requires example:bar and example:foo, we can conclude that your project's
      requirements are unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip
        locking and syncing.
```

So `project:group`

---

_@zanieb reviewed on 2025-06-11 20:56_

---

_@zanieb reviewed on 2025-06-11 20:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:8957 on 2025-06-11 20:57_

Should we say "Project `example` has ..."? I think that's more consistent with some other errors.

---

_@zanieb reviewed on 2025-06-11 20:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:518 on 2025-06-11 20:58_

This isn't used, should we include it? Should it be `>=3.9` instead?

---

_@zanieb reviewed on 2025-06-11 20:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:511 on 2025-06-11 20:58_

Why wouldn't we select 3.11 since that's first and matches the range?

---

_@zanieb reviewed on 2025-06-11 20:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:511 on 2025-06-11 20:59_

(Spoiler, the snapshot says we do select 3.11)

---

_@zanieb reviewed on 2025-06-11 20:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:545 on 2025-06-11 20:59_

This doesn't tell us anything because we selected 3.11 above too.

---

_@zanieb reviewed on 2025-06-11 20:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:578 on 2025-06-11 20:59_

How is this intended to be different from the above case?

---

_@zanieb reviewed on 2025-06-11 21:00_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:664 on 2025-06-11 21:00_

This shouldn't be a workspace, still confused about this.

---

_@zanieb reviewed on 2025-06-11 21:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock_target.rs`:223 on 2025-06-11 21:05_

Should we talk through this? What's the question? We're figuring out the `requires-python` to use for locking the whole workspace? This is the intersection of all the workspace member values? (It's confusing that the documentation for `find_requires_python` says "union"). I don't think we want to narrow it further if a dependency group only supports a subset of Python versions.

---

_@zanieb reviewed on 2025-06-11 21:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock_target.rs`:223 on 2025-06-11 21:06_

(tbh I'm not really sure why we'd want to narrow if a workspace member supports a subset either, I think the workspace member should just not be installable outside its supported versions, but I don't know if we support that concept today)

---

_@zanieb reviewed on 2025-06-11 21:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:204 on 2025-06-11 21:06_

Noted elsewhere, but I don't think we should invent a new format here.

---

_@zanieb reviewed on 2025-06-11 21:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:383 on 2025-06-11 21:09_

I'm not sure if this TODO makes sense in this context anymore.

---

_@zanieb reviewed on 2025-06-11 21:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/version.rs`:383 on 2025-06-11 21:09_

I think it's about `install_options`?

---

_@zanieb reviewed on 2025-06-11 21:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-11 21:10_

Why not? What happens if you do `uv python pin 3.12` but a dependency group requires `>=3.13`? What if it's the `dev` group?

---

_@zanieb reviewed on 2025-06-11 21:12_

---

_Review comment by @zanieb on `crates/uv-distribution/src/metadata/requires_dist.rs`:149 on 2025-06-11 21:12_

Can we error eagerly if you use both?

---

_@zanieb reviewed on 2025-06-11 21:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4836 on 2025-06-11 21:13_

Ah okay thanks for clarifying. Probably worth cleaning up.

---

_@Gankra reviewed on 2025-06-11 21:16_

---

_Review comment by @Gankra on `crates/uv/tests/it/venv.rs`:578 on 2025-06-11 21:16_

It swaps the order of which of two sources of constraints is the dominant one, to make sure both sources are considered.

---

_@Gankra reviewed on 2025-06-11 21:18_

---

_Review comment by @Gankra on `crates/uv/tests/it/venv.rs`:511 on 2025-06-11 21:18_

Oh wild, this is based on the preexisting test above it, I never noticed it purposely scrambled them.

---

_@Gankra reviewed on 2025-06-11 21:30_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/lock_target.rs`:223 on 2025-06-11 21:30_

Yeah the way this value gets used is very strange, now that I've had more time to think about this I'm fairly confident we want the impl I have checked in.

---

_@zanieb reviewed on 2025-06-11 21:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/venv.rs`:578 on 2025-06-11 21:33_

How does it demonstrate anything if the result doesn't change? 🤔 

---

_@Gankra reviewed on 2025-06-11 21:33_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:383 on 2025-06-11 21:33_

Yeah the comment can probably be killed, I think it's just gesturing to the confusing nature of all these `project` commands invoking sync as subroutine when syncing can require a million different settings.

---

_@Gankra reviewed on 2025-06-11 21:34_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-11 21:34_

I believe my concern here was if we do this we'll have to add all the dependency-group flags to `pin`

---

_@zanieb reviewed on 2025-06-11 21:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-11 21:37_

I don't think you _need_ to add the group flags, per-say. You almost definitely shouldn't. It's just used to power a warning, right? You probably don't want your pinned Python version to be incompatible with your default groups. I'm not even sure you want it to be incompatible with your non-default groups? because then `--group foo` will fail without an explicit `-p <version>` which is super annoying.

---

_@Gankra reviewed on 2025-06-11 23:41_

---

_Review comment by @Gankra on `crates/uv/tests/it/venv.rs`:578 on 2025-06-11 23:41_

`uv venv` aiui completely clobbers the old venv, so each one of these invocations is independent (I've now fixed the issue where 3.11 was first so now these constraints actually do something non-trivial)

---

_@Gankra reviewed on 2025-06-11 23:46_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4837 on 2025-06-11 23:46_

Hmm in this case with other suggestions we'll end up with:

```
 - project: >=3.9, <3.10 
 - project:group: >=3.10
```

Which looks a little weird to me but actually it's growing on me

---

_@Gankra reviewed on 2025-06-11 23:51_

---

_Review comment by @Gankra on `crates/uv/tests/it/venv.rs`:664 on 2025-06-11 23:51_

Previously this error was only possible within a workspace because it required multiple packages, now a single package can contradict itself... maybe "Found conflicting Python requirements"?

---

_@Gankra reviewed on 2025-06-11 23:58_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-11 23:58_

So I don't think we should ever use the intersection of --all-groups without the user asking, because although it's very dubious for this to come up, I think it should be allowed for two groups to have conflicting requires-pythons.

There is one place in the code where assert_pin_compatible_project is `?`ed instead of immediately discarded as a warning:

```
if let Some(virtual_project) = &virtual_project {
        if let Some(request_version) = pep440_version_from_request(&request) {
             assert_pin_compatible_with_project(
```

---

_@Gankra reviewed on 2025-06-12 00:03_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4730 on 2025-06-12 00:03_

Thanks! (Seems there's some part of the codebase not doing this..)

---

_@Gankra reviewed on 2025-06-12 00:35_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4727 on 2025-06-12 00:35_

In general even a single package is a workspace, but the pre-existing error previously was only relevant with a non-trivial workspace.

So this is a funny rube-goldberg situation:

* we compute all the requires_pythons with precise source info included
* then we intersect them together and throw out all that info
* then we figure out our python version (in this case with an explicit request)
* then we check if that version matches our intersected requires_python
* oh no it doesn't, ok let's try to come up with a Useful error message
* oh look this package on its own has a requires-python that's compatible, you could just install that on its own?

And because we specifically recommend `uv pip install` the instructions are actually accidentally correct? Because it's an extremely obtuse way to express "disable the dependency-groups of the package". Fixing up this error is a bit annoying because all the context gets dropped... I'm trying to see if it's easy to hold onto that context.

---

_@Gankra reviewed on 2025-06-12 00:58_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4727 on 2025-06-12 00:58_

Ok the more I look at this the more I'm extremely dubious of this "extra helpful" diagnostic we've had for quite a while: https://github.com/astral-sh/uv/blob/f5305653238378376cd33f1a00d6b9eb511bef36/crates/uv/src/commands/project/mod.rs#L467-L483

I think maybe it dates from a period where validate_project_requires_python was less pervasively invoked by everything...

---

_@Gankra reviewed on 2025-06-12 02:12_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:349 on 2025-06-12 02:12_

The polarity changes here: instead of saying "btw here's a random Good package" we now go "btw here's all the Bad packages (and/or groups)".

---

_@Gankra reviewed on 2025-06-12 02:13_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:4727 on 2025-06-12 02:13_

Now we more coherently point at the group as problematic, and don't suggest some cryptic workaround.

---

_@Gankra reviewed on 2025-06-12 02:17_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:349 on 2025-06-12 02:17_

I've completely removed the "btw here's a random Good package" message because... it seemed very confusing and random, especially since it could appear on basically any random uv command.

---

_@Gankra reviewed on 2025-06-12 02:19_

---

_Review comment by @Gankra on `crates/uv-distribution/src/metadata/requires_dist.rs`:149 on 2025-06-12 02:19_

Hmm... like "you're using Modern dependency-groups features, please migrate off Legacy dependency-groups?" even if there's no actual interaction with how they're using them?

---

_Review comment by @konstin on `crates/uv-workspace/Cargo.toml`:47 on 2025-06-12 08:26_

```suggestion
uv-configuration = { workspace = true }
```

this dep should go into the uv section

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 08:30_

This reads oddly, we're first saying it's the project's requirement, then we're saying in the next sentence the requirement is from foo. Can we merge the two sentences?

---

_Review comment by @konstin on `crates/uv/tests/it/run.rs`:4678 on 2025-06-12 08:30_

nice!

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:20664 on 2025-06-12 08:34_

In the new tests, are we covering a scenario where a dependency group package couldn't be installed on an older python version, so that we have a realistic use case in the tests that would fail without the requires-python?

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:204 on 2025-06-12 08:42_

Not your fault, but uv will happily do

```
uv add --group science numpy --script main.py
```

to create

```python
# /// script
# requires-python = ">=3.13"
#
# [dependency-groups]
# science = [
#     "numpy",
# ]
# ///
```

it doesn't like

```
uv sync --group science --script main.py
```

otoh, but there's some gap here; maybe something for a separate PR.

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject.rs`:356 on 2025-06-12 09:17_

nit: the other docstrings end with a dot.

```suggestion
    /// Additional settings for `dependency-groups`.
```

---

_@konstin reviewed on 2025-06-12 09:19_

---

_@Gankra reviewed on 2025-06-12 12:51_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/add.rs`:204 on 2025-06-12 12:51_

Yeah that sounds like a bug in `uv add`, nowhere in the code believes this can happen.

---

_@Gankra reviewed on 2025-06-12 13:02_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 13:02_

Hmm maybe

error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the project's Python requirement: `>=3.11, <4` (from `foo`).

---

_@Gankra reviewed on 2025-06-12 13:05_

---

_Review comment by @Gankra on `crates/uv/tests/it/lock.rs`:20664 on 2025-06-12 13:05_

Hmm what kind of "can't be installed" are you picturing? A registry package with a requires-python?

---

_@zanieb reviewed on 2025-06-12 13:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:204 on 2025-06-12 13:07_

https://github.com/astral-sh/uv/issues/13988

---

_@zanieb reviewed on 2025-06-12 17:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 17:46_

> So I don't think we should ever use the intersection of --all-groups without the user asking, because although it's very dubious for this to come up, I think it should be allowed for two groups to have conflicting requires-pythons.

I agree, but we might want do something like use the intersection of all then fallback to the default if that was empty? Especially for cases where we're just powering a warning. The intent of groups `requires-python` is mostly about narrowing, rather than conflicting.

> There is one place in the code where assert_pin_compatible_project is ?ed instead of immediately discarded as a warning

What is this case though?

---

_@zanieb reviewed on 2025-06-12 17:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:4727 on 2025-06-12 17:51_

Yeah it seems like we may want to drop that and open an issue to follow up on it?

> In general even a single package is a workspace

Even if this is true internally, we shouldn't leak that to the user. Most users don't need or use workspaces.

---

_@zanieb reviewed on 2025-06-12 17:53_

---

_Review comment by @zanieb on `crates/uv-workspace/src/dependency_groups.rs`:178 on 2025-06-12 17:53_

I think this sounds right, but the comment in the code is confusing.

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 17:54_

What's `foo` here?

---

_@zanieb reviewed on 2025-06-12 17:54_

---

_@Gankra reviewed on 2025-06-12 17:55_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 17:55_

> What is this case though?

You're in any kind of project/workspace and you requested a python version. We then refuse to proceed if the workspace's intersected requires-python conflicts with that (or if the workspace has an empty intersection). I suppose it's not entirely unreasonable to error if *any* of your dependency-groups would be incompatible with it?

---

_@Gankra reviewed on 2025-06-12 17:56_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 17:56_

Package name (can also be `mypackage:group`)

---

_@zanieb reviewed on 2025-06-12 17:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 17:59_

This is sort of a landmine :D we'll want test cases covering the interactions here.

The problem is, once you pin a Python version, it's the default request. So _every_ project command will fail if it's not compatible with the selected groups? If it's incompatible with the `requires-python` of the project, yeah we should hard error because it's _never_ usable. If it's incompatible with the default groups, we have a big problem and.. should probably error too? If it's incompatible with some opt-in groups, they'll still have to use `-p` to use those groups, which is bad? but probably a warning. 

I think we'll need to call this function with various levels of groups activated to get the full warning / error experience we need.

---

_@Gankra reviewed on 2025-06-12 18:05_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 18:05_

Fair, sadness.

---

_@Gankra reviewed on 2025-06-12 18:06_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 18:06_

I feel like we could ship with "no it's just an error if --all-groups fails" because this is a new opt-in feature, and loosen it if people actually complain?

---

_@zanieb reviewed on 2025-06-12 18:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 18:07_

I think it's really weird to say "(from `foo`)" in a single project workspace (where `foo` is the name of the project). It's too confusing.

I'd maybe say...

For a project:

> error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the project's Python requirement: >=3.11, <4

Keep it simple for the normal case. Adding the project name here isn't helpful. If anything, I'd append

> (from `project.requires-python`)

For a group in a project:

> error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the project's Python requirement: >=3.11, <4 (from group `foo`)

I don't love this still, because what does it mean for the Python requirement to be from group `foo`? Is that _just_ foo's Python requirement? Maybe something like:

> error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the group `foo`'s Python requirement: >=3.11, <4 (from `tool.uv.dependency-groups.foo.requires-python`)

For workspaces with multiple projects, it gets more complicated?

> error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the workspace's Python requirement: >=3.11, <4 (from member `foo`).

Once again, I don't know what it means to be "from member", just going off the language you had. It might make more sense as:

> error: The requested interpreter resolved to Python 3.9.[X], which is incompatible with the workspace member `foo`'s Python requirement: >=3.11, <4

And it gets more complicated for groups of workspace members, but I think we need to be on the same page about what `from` means in order to talk about a message.


---

_@zanieb reviewed on 2025-06-12 18:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-12 18:09_

I think I'd be wary of that, it'd make more sense as a warning.

---

_@Gankra reviewed on 2025-06-12 18:52_

---

_Review comment by @Gankra on `crates/uv/tests/it/run.rs`:136 on 2025-06-12 18:52_

context: if you have a bunch of requires-pythons and you intersect them together, and then you ask "is this SPECIFIC version in that intersection", and the answer comes back "no": there must exist at least one requires-python that, on its own, excluded that version.

(As an example, if you have 4 sources that specify `>= 3.10`, `>=3.11`, `>= 3.12`, and `<3.13` you get an intersection of `>= 3.12, <3.13` and your request for "3.10" fails. We can then go "hey the things that specify `>=3.11` and `>=3.12` are the ones that are specifically incompatible".)

I'm making these error messages print out *all* requires-python (sources) that excluded the requested python. There are a few cases of this:

* there's only one requires-python and it excluded it, simple print that one
* there's lots of requires-pythons, but one was more extreme and excluded it, simple print just that one
* there's lots of requires-pythons, many excluded it, print all of those

The idea here is the user can then go:

* oh i didn't need that thing => disable it somehow
* ok but I needed that thing =>
  * change the version they request 
  * *OR* change the requires-python bound

---

_@konstin reviewed on 2025-06-12 19:42_

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:20664 on 2025-06-12 19:42_

yeah, like we have with sphinx in the issue

---

_@konstin reviewed on 2025-06-13 09:59_

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:20664 on 2025-06-13 09:59_

As far as i can tell, in the current tests, you could remove the `requires-python`, and it would continue working, so it would be good to have a case that breaks if the `requires-python` aren't there or aren't respected because the packages themselves don't support it, to bake the motivating use case into the test case. if we don't have the packages for this already, packse is great for modelling this.

---

_@Gankra reviewed on 2025-06-13 18:49_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/pin.rs`:326 on 2025-06-13 18:49_

Note: The python pin behaviour is messy enough that we're going to punt this to a followup.

---

_@Gankra reviewed on 2025-06-13 19:21_

---

_Review comment by @Gankra on `crates/uv/tests/it/sync.rs`:400 on 2025-06-13 19:21_

This is now an opportunity for us to emit a Better diagnostic suggesting group-requires-python (let's make this a followup...).

---

_@zanieb approved on 2025-06-13 21:50_

---

_Merged by @Gankra on 2025-06-13 22:04_

---

_Closed by @Gankra on 2025-06-13 22:04_

---

_Branch deleted on 2025-06-13 22:04_

---
