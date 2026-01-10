```yaml
number: 16078
title: "Deduplicate marker-specific dependencies in `uv pip tree` output"
type: pull_request
state: merged
author: terror
labels: []
assignees: []
merged: true
base: main
head: aggregate-tree-requirements
created_at: 2025-09-30T18:59:52Z
updated_at: 2025-10-01T16:05:08Z
url: https://github.com/astral-sh/uv/pull/16078
synced_at: 2026-01-10T06:36:15Z
```

# Deduplicate marker-specific dependencies in `uv pip tree` output

---

_Pull request opened by @terror on 2025-09-30 18:59_

Resolves https://github.com/astral-sh/uv/issues/16067

When we build the dependency graph we add an edge per `Requires-Dist`. If a package publishes multiple marker-guarded requirements (like pylint’s `dill>=…` for different Python versions), more than one marker can evaluate to true at runtime, which gives us several edges pointing to the same installed package node.

To avoid printing the package multiple times, we gather all edges targeting that node, pass them through a `Cursor`, and aggregate their requirement details before we print the dependency line. That aggregation does two things:
  1. If any edge carries a URL, we return that URL immediately.
  2. Otherwise we merge all version specifiers into one canonical PEP 440 string.

I've added an integration test, namely `cargo test -p uv --test it --features pypi -- no_duplicate_dependencies_with_markers` exercises the new snapshot that installs pylint (with the real dill duplication scenario present in the original issue) and asserts the tree output, both with and without `--show-version-specifiers`, now shows a single dill entry with the merged constraint.

---

_Marked ready for review by @terror on 2025-09-30 19:38_

---

_Comment by @zanieb on 2025-09-30 19:48_

Thanks for the pull request! Let me double check what we actually do at resolution-time, I'm not sure if we actually merge the specifiers as done here and I want to make sure we're doing the same thing.

---

_@zanieb reviewed on 2025-09-30 19:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_tree.rs`:1168 on 2025-09-30 19:49_

Can you use `new_with_versions` and add `3.11` too? Then you can add a case that uses `-p 3.11` to verify the version specifiers are correct there?

---

_@zanieb reviewed on 2025-09-30 19:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_tree.rs`:1168 on 2025-09-30 19:50_

Separately, why did you use an exclude-newer in the future? That'll cause the snapshot to be unstable.

---

_@zanieb reviewed on 2025-09-30 19:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_tree.rs`:1171 on 2025-09-30 19:51_

We might want to add a local package, e.g., to `scripts/packages` which has repeated `Requires-Dist` values. It's _okay_ to use pylint, but it'll make the test slower. We can track this in a separate issue if you prefer.

---

_Review comment by @terror on `crates/uv/tests/it/pip_tree.rs`:1168 on 2025-09-30 19:59_

I had wanted to test the specific versions in the original issue, which were published after the default `EXCLUDE_NEWER` value of `2024-03-25T00:00:00Z`. I think we should go with the [approach](https://github.com/astral-sh/uv/pull/16078#discussion_r2392670905) you mention below for this test however.

---

_@terror reviewed on 2025-09-30 19:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-09-30 20:11_

I'm a little wary of this. We almost certainly already have logic for combining version specifiers, though I'm not sure we can use it here, e.g.

https://github.com/astral-sh/uv/blob/3f83390e342dcb6fccc449335c15d1255885ad20/crates/uv-pep440/src/version_ranges.rs#L13-L22

It does look pretty clean though and is nicer than emitting redundant ranges, e.g., `>=3.6, >=3.7`.

@konstin might have some thoughts.

---

_@zanieb reviewed on 2025-09-30 20:11_

---

_Review requested from @konstin by @zanieb on 2025-09-30 20:18_

---

_Review comment by @terror on `crates/uv/tests/it/pip_tree.rs`:1171 on 2025-09-30 21:03_

I've added a local package `marker-dup` (alongside another target package) specifically for the test, making it faster.

---

_@terror reviewed on 2025-09-30 21:03_

---

_@terror reviewed on 2025-09-30 21:22_

---

_Review comment by @terror on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-09-30 21:22_

From what I understand, this is a lossy conversion, e.g. once we intersect the specifiers into intervals for PubGrub we lose the exact shape of the original constraints, and there isn’t a reliable way to turn an arbitrary range back into a finite set of PEP 440 specifiers.

We could instead move the specifier-level deduplication that tightens bounds into the PEP 440 crate, exposing it for other potential use-cases and decluttering the `pip tree` implementation. What do you think?

---

_@zanieb reviewed on 2025-09-30 22:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-09-30 22:28_

Yeah it makes sense that the PubGrub range would lose the original constraints.

I think I'll want to hear from @konsti, but either having it here until we have another use-case or moving it to the PEP 440 crate seems reasonable.

---

_Review comment by @konstin on `scripts/packages/dup-target/src/dup_target.egg-info/dependency_links.txt`:1 on 2025-10-01 09:12_

`egg.info` files should not be checked into the Git repository.

---

_Review comment by @konstin on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-10-01 09:24_

Ideally we'd be using `Ranges<Version>`, which gives us a canonical representation. The problem is that iirc we don't have a function yet for that conversion that handles prereleases correctly. There's another catch to this that while `>=2.0.0` is technically larger than `>=2.0.0a1`, the latter allows prereleases while the former doesn't.

I wouldn't put too much effort into making simplifications on `VersionSpecifiers` work, `Ranges` is a much better representation to work with.

---

_Review comment by @konstin on `crates/uv/tests/it/pip_tree.rs`:1171 on 2025-10-01 09:30_

I'd create this package in the test itself, most other tests do that too, searching for `.touch()` and `context.init()` should show them. This for example works:

```toml
[project]
name = "debug"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "sniffio>=1.0.0; python_version >= '3.11'",
  "sniffio>=1.0.1; python_version >= '3.12'",
  "sniffio>=1.0.2; python_version >= '3.13'",
]

[build-system]
requires = ["uv_build>=0.8.22,<10000"]
build-backend = "uv_build"
```

---

_Review comment by @konstin on `crates/uv/src/commands/pip/tree.rs`:506 on 2025-10-01 09:43_

`fold` usually harder to read than the imperative versions, I'd use a `for` loop here and early `return` if it encounters a `Url`.

---

_@konstin reviewed on 2025-10-01 09:44_

---

_@zanieb reviewed on 2025-10-01 13:40_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_tree.rs`:1171 on 2025-10-01 13:40_

Ah fair point.

---

_@zanieb reviewed on 2025-10-01 13:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-10-01 13:41_

I think the amount of effort here for simplifying version specifiers seems pretty reasonable for user display if the only alternative is `Ranges<Version>`

---

_Review comment by @terror on `scripts/packages/dup-target/src/dup_target.egg-info/dependency_links.txt`:1 on 2025-10-01 15:00_

I've removed this in favour of creating the package inline within the test!

---

_@terror reviewed on 2025-10-01 15:00_

---

_@terror reviewed on 2025-10-01 15:23_

---

_Review comment by @terror on `crates/uv/tests/it/pip_tree.rs`:1171 on 2025-10-01 15:23_

I've refactored the test to create the package inline, which seems to be much simpler than persisting multiple packages to disk!

---

_Review comment by @terror on `crates/uv/src/commands/pip/tree.rs`:564 on 2025-10-01 15:34_

> Ideally we'd be using `Ranges<Version>`, which gives us a canonical representation. The problem is that iirc we don't have a function yet for that conversion that handles prereleases correctly. There's another catch to this that while `>=2.0.0` is technically larger than `>=2.0.0a1`, the latter allows prereleases while the former doesn't.
> 
> I wouldn't put too much effort into making simplifications on `VersionSpecifiers` work, `Ranges` is a much better representation to work with.

Can we track this in a separate issue? It seems like a bigger lift, and I wouldn't mind looking into it as a follow up.

---

_@terror reviewed on 2025-10-01 15:34_

---

_@zanieb approved on 2025-10-01 16:01_

---

_Merged by @zanieb on 2025-10-01 16:01_

---

_Closed by @zanieb on 2025-10-01 16:01_

---

_Branch deleted on 2025-10-01 16:05_

---
