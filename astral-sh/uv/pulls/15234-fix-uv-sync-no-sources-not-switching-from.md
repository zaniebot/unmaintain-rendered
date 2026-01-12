```yaml
number: 15234
title: " Fix `uv sync --no-sources` not switching from editable to registry installations"
type: pull_request
state: merged
author: yumeminami
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/sync-no-sources-editable-switch
created_at: 2025-08-12T09:25:18Z
updated_at: 2025-09-17T11:35:32Z
url: https://github.com/astral-sh/uv/pull/15234
synced_at: 2026-01-12T16:11:38Z
```

#  Fix `uv sync --no-sources` not switching from editable to registry installations

---

_@yumeminami_

 ## Summary

Fixes issue #15190 where `uv sync --no-sources` fails to switch from editable to registry package installations. The problem occurred because the installer's satisfaction check didn't consider the `--no-sources` flag when determining if an existing editable installation was compatible with a registry requirement.

## Solution

Modified `RequirementSatisfaction::check()` to reject non-registry installations when `SourceStrategy::Disabled` and the requirement is from registry. Added `SourceStrategy` parameter threading through the entire call chain from commands to the satisfaction check to ensure consistent behavior between `uv sync --no-sources` and `uv pip install --no-sources`.

---

_Comment by @zanieb on 2025-08-12 13:00_

Hey thanks for taking a swing at this!

Naively, I think I'd expect this to be implemented in `SitePackages` instead? e.g.,

https://github.com/astral-sh/uv/blob/c19a294a48351ca84f18149fd6bcbf17d9e20bd9/crates/uv-installer/src/site_packages.rs#L291-L298

https://github.com/astral-sh/uv/blob/c19a294a48351ca84f18149fd6bcbf17d9e20bd9/crates/uv-installer/src/site_packages.rs#L386-L393

Did you look at that? Does that not work for some reason?

---

_Comment by @yumeminami on 2025-08-13 02:47_

@zanieb 

 After reviewing the `uv sync `call chain, I found that it does not use `SitePackages::satisfies_spec()` or
  `satisfies_requirements()`. Instead, it follows this path:

  `uv sync` →  `operations::install()` → `Planner::build()` → `RequirementSatisfaction::check()`

  The bug is in `RequirementSatisfaction::check()` at https://github.com/astral-sh/uv/blob/c19a294a48351ca84f18149fd6bcbf17d9e20bd9/crates/uv-installer/src/satisfies.rs#L33-L38

  Why Not Pass SourceStrategy?

  Initially, I considered passing `SourceStrategy` to `RequirementSatisfaction::check()`, but this would require:

  1. Extensive API changes: Modifying function signatures across multiple layers
  2. High impact: Affecting many unrelated code paths
  3. Complex propagation: Threading the parameter through the entire call chain

---

_Comment by @yumeminami on 2025-08-13 03:18_

@zanieb 

same problem use `uv pip install --no-sources`

```bash
(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ cat pyproject.toml             
[project]
name = "test_no_sources"
version = "0.0.1"

dependencies = ["git_mcp_server"]

[tool.uv.sources]
git_mcp_server = { path = "./git_mcp", editable = true }


(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ head git_mcp/pyproject.toml
[project]
name = "git_mcp_server"
version = "0.1.9"
description = "Git MCP Server - Unified command-line tool for managing Git repositories across GitHub and GitLab"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "python-gitlab>=4.4.0",
    "PyGithub>=2.1.0",
    "click>=8.1.0",
    
   
(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ uv pip install --no-sources git_mcp_server
Resolved 64 packages in 48ms
Installed 1 package in 7ms
 + git-mcp-server==0.1.8
(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ uv pip install -e .                       
Resolved 65 packages in 24ms
      Built test-no-sources @ file:///Users/wingmunfung/workspace/uv/test_uv_pip_no_sources
Prepared 1 package in 520ms
Uninstalled 2 packages in 4ms
Installed 2 packages in 3ms
 - git-mcp-server==0.1.8
 + git-mcp-server==0.1.9 (from file:///Users/wingmunfung/workspace/uv/test_uv_pip_no_sources/git_mcp)
 ~ test-no-sources==0.0.1 (from file:///Users/wingmunfung/workspace/uv/test_uv_pip_no_sources)
 
 
(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ uv pip install --no-sources git_mcp_server
Audited 1 package in 15ms


(test_uv_pip_no_sources) ➜  test_uv_pip_no_sources git:(fix/sync-no-sources-editable-switch) ✗ uv pip show git_mcp_server                
Name: git-mcp-server
Version: 0.1.9
Location: /Users/wingmunfung/workspace/uv/test_uv_pip_no_sources/.venv/lib/python3.13/site-packages
Editable project location: /Users/wingmunfung/workspace/uv/test_uv_pip_no_sources/git_mcp
Requires: click, gitpython, httpx, keyring, mcp, pre-commit, pydantic, pygithub, python-gitlab, pyyaml, rich, tool
Required-by: test-no-sources
(test_uv_pip_no_sources) ➜  
```

I think the `git-mcp-server` version should be  `0.1.8` while I run `v pip install --no-sources git_mcp_server`

---

_Comment by @zanieb on 2025-08-13 03:52_

Yes sorry, `RequirementSatisfaction::check` and the `Planner::build` methods are relevant too.

I'm not sure if we need to propagate the source strategy? That should be reflected in the resolution?

I think we should be able to tell if an installed distribution matches the resolution. Naively, I'd image we want something like this?

```diff
diff --git a/crates/uv-installer/src/satisfies.rs b/crates/uv-installer/src/satisfies.rs
index b7e824202..48709e2cc 100644
--- a/crates/uv-installer/src/satisfies.rs
+++ b/crates/uv-installer/src/satisfies.rs
@@ -31,7 +31,9 @@ impl RequirementSatisfaction {
         match source {
             // If the requirement comes from a registry, check by name.
             RequirementSource::Registry { specifier, .. } => {
-                if specifier.contains(distribution.version()) {
+                if matches!(distribution, InstalledDist::Registry { .. })
+                    && specifier.contains(distribution.version())
+                {
                     return Self::Satisfied;
                 }
                 Self::Mismatch
```

---

_Comment by @yumeminami on 2025-08-13 12:14_

@zanieb 

Thank you for your feedback.

After applying your modification, I was able to install the package again using `uv sync --no-sources` instead of getting no output.

However, running `uv pip install --no-sources` will throws an exception.

Upon closer inspection, I found that the call chains for `sync` and `pip install` differ. In particular, pip install uses a `CandidateSelector`, whose `CandidateSelector::get_installed()` strategy is inconsistent with the matching logic in `RequirementSatisfaction::check()`.

This mismatch causes the resolver to treat the package as `ResolvedDist::Installed`(does not check it is a Url or a Registry), while `RequirementSatisfaction::check()` flags it as **mismatched**—leading to a panic.

```
(test_uv_sync) ➜ test_uv_sync uv-dev pip install --no-sources anyio -v
DEBUG uv 0.8.9+2 (250c3a746 2025-08-13)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.13.2-macos-aarch64-none` at `/Users/wingmunfung/test_uv_sync/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.13.2 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG Registry check: distribution=Url(InstalledDirectUrlDist { name: PackageName("anyio"), version: "4.3.0", direct_url: LocalDirectory { url: "file:///Users/wingmunfung/test_uv_sync/local_dep", dir_info: DirInfo { editable: Some(true) }, subdirectory: None }, url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/Users/wingmunfung/test_uv_sync/local_dep", query: None, fragment: None }, editable: true, path: "/Users/wingmunfung/test_uv_sync/.venv/lib/python3.13/site-packages/anyio-4.3.0.dist-info", cache_info: Some(CacheInfo { timestamp: Some(Timestamp(SystemTime { tv_sec: 1755059554, tv_nsec: 377749146 })), commit: None, tags: None, env: {}, directories: {"src": None} }) }), specifier=VersionSpecifiers([]), version="4.3.0"
DEBUG At least one requirement is not satisfied: anyio
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13.2
DEBUG Adding direct dependency: anyio*
DEBUG Found stale response for: https://pypi.org/simple/anyio/
DEBUG Sending revalidation request for: https://pypi.org/simple/anyio/
DEBUG Found not-modified response for: https://pypi.org/simple/anyio/
DEBUG Found installed version of anyio==4.3.0 (from file:///Users/wingmunfung/test_uv_sync/local_dep) that satisfies *
DEBUG Searching for a compatible version of anyio (*)
DEBUG Found installed version of anyio==4.3.0 (from file:///Users/wingmunfung/test_uv_sync/local_dep) that satisfies *
DEBUG Selecting: anyio==4.3.0 [installed] (installed)
DEBUG Tried 1 versions: anyio 1
DEBUG marker environment resolution took 0.248s
Resolved 1 package in 251ms
DEBUG Registry check: distribution=Url(InstalledDirectUrlDist { name: PackageName("anyio"), version: "4.3.0", direct_url: LocalDirectory { url: "file:///Users/wingmunfung/test_uv_sync/local_dep", dir_info: DirInfo { editable: Some(true) }, subdirectory: None }, url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/Users/wingmunfung/test_uv_sync/local_dep", query: None, fragment: None }, editable: true, path: "/Users/wingmunfung/test_uv_sync/.venv/lib/python3.13/site-packages/anyio-4.3.0.dist-info", cache_info: Some(CacheInfo { timestamp: Some(Timestamp(SystemTime { tv_sec: 1755059554, tv_nsec: 377749146 })), commit: None, tags: None, env: {}, directories: {"src": None} }) }), specifier=VersionSpecifiers([VersionSpecifier { operator: Equal, version: "4.3.0" }]), version="4.3.0"
DEBUG Requirement installed, but mismatched: 
Installed: Url(InstalledDirectUrlDist { name: PackageName("anyio"), version: "4.3.0", direct_url: LocalDirectory { url: "file:///Users/wingmunfung/test_uv_sync/local_dep", dir_info: DirInfo { editable: Some(true) }, subdirectory: None }, url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/Users/wingmunfung/test_uv_sync/local_dep", query: None, fragment: None }, editable: true, path: "/Users/wingmunfung/test_uv_sync/.venv/lib/python3.13/site-packages/anyio-4.3.0.dist-info", cache_info: Some(CacheInfo { timestamp: Some(Timestamp(SystemTime { tv_sec: 1755059554, tv_nsec: 377749146 })), commit: None, tags: None, env: {}, directories: {"src": None} }) }) 
Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "4.3.0" }]), index: None, conflict: None }

thread 'main2' panicked at crates/uv-installer/src/plan.rs:150:17:
internal error: entered unreachable code: Installed distribution could not be Found in site-packages: anyio==4.3.0 (from file:///Users/wingmunfung/test_uv_sync/local_dep)
Note: Run with `RUST_BACKTRACE=1` environment variable to display a backtrace
DEBUG Released lock at `/Users/wingmunfung/test_uv_sync/.venv/.lock`

Thread 'main' panicked at /Users/wingmunfung/workspace/uv/crates/uv/src/lib.rs:2300:10:
Tokio executor failed, was there a panic?: Any { .. }
```



---

_Comment by @yumeminami on 2025-08-13 14:44_

https://github.com/astral-sh/uv/blob/78c8c711fa6d86b95f3e4b749da4573c01d2aae0/crates/uv-installer/src/plan.rs#L94-L102

Doesn’t the log above violate the assumption in the comment https://github.com/astral-sh/uv/pull/15234#issuecomment-3183609624?

```
DEBUG Found installed version of anyio==4.3.0 (from file:///Users/wingmunfung/test_uv_sync/local_dep) that satisfies *
DEBUG Selecting: anyio==4.3.0 [installed] (installed)
DEBUG Requirement installed, but mismatched: 
Installed: Url(InstalledDirectUrlDist { name: PackageName("anyio"), version: "4.3.0", direct_url: LocalDirectory { url: "file:///Users/wingmunfung/test_uv_sync/local_dep", dir_info: DirInfo { editable: Some(true) }, subdirectory: None }, url: DisplaySafeUrl { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/Users/wingmunfung/test_uv_sync/local_dep", query: None, fragment: None }, editable: true, path: "/Users/wingmunfung/test_uv_sync/.venv/lib/python3.13/site-packages/anyio-4.3.0.dist-info", cache_info: Some(CacheInfo { timestamp: Some(Timestamp(SystemTime { tv_sec: 1755059554, tv_nsec: 377749146 })), commit: None, tags: None, env: {}, directories: {"src": None} }) }) 
Requested: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "4.3.0" }]), index: None, conflict: None }
```

---

_Comment by @yumeminami on 2025-08-14 07:48_

@zanieb 

Could you help me look into this issue?

---

_Comment by @zanieb on 2025-08-14 12:30_

Hey, I'll look when I get a chance — I've been a bit busy and don't know the answer off the top of my head so I'll need to dig into the problem.

---

_Comment by @charliermarsh on 2025-08-16 22:29_

I think that comment is probably wrong. I think `CandidateSelector::select` _can_ return `InstalledDirectUrlDist` if you have an already-installed URL that matches a user requirement (e.g., you have `anyio @ 4.3.0` installed from Git, and you run `uv pip install anyio`).

I think the error you're seeing is because you changed the logic in the install plan, but you need to make a corresponding change in the resolver, such that when resolving, we _don't_ choose the already-installed URL distribution if the user didn't request a URL. In other words: you need to reflect the change in both the install plan _and_ the resolver, and modifying `satisfies` only affects the install plan.

It's not totally clear to me whether we want that behavior, though. It's clear for the `uv sync` case, but like, if we make this change as described, then `uv pip install git+https://github.com/agronholm/anyio` followed by `uv pip install anyio` would uninstall the Git-based `anyio` and then reinstall it from PyPI. That would be a breaking change, so we probably make sure we want to do that before finishing the PR.

(Alternatively, if we could somehow make this change _only_ for the `uv sync` case, that'd be easier to ship.)


---

_Comment by @yumeminami on 2025-08-17 17:24_

@charliermarsh 

My current change is limited to handling `uv sync --no-sources` only:

1. Check whether the `SourceStrategy` is false.
2. Collect the registry packages from the resolution.
3. Check for already `InstalledDist::Url(_)`/`InstalledDist::LegacyEditable(_)` packages.
4. Mark them for reinstallation.

Do you think this scoped fix is small enough for now?

I also experimented with modifying the resolver. Specifically, I tried passing `SourceStrategy` into the `CandidateSelector`, and alternatively, filtering out `InstalledDist::Url` and `InstalledDist::LegacyEditable` in `CandidateSelector::get_installed()` so they’re no longer considered as installed. This does solve the issue for `pip install --no-sources`, but it comes with some drawbacks:

- `CandidateSelector` is used in many modules, so the change could have unintended consequences.
- It requires passing parameters deeply, which means modifying many call sites.
- It affects a large number of unit tests.

---

_Comment by @charliermarsh on 2025-08-21 23:08_

@yumeminami -- Sorry, will get back to you soon, this is on my list.

---

_Review requested from @charliermarsh by @charliermarsh on 2025-08-22 10:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-22 10:23_

---

_Label `bug` added by @charliermarsh on 2025-08-22 10:23_

---

_Comment by @charliermarsh on 2025-08-22 11:45_

@yumeminami -- What do you think of https://github.com/astral-sh/uv/pull/15450? It's a slightly different approach whereby the `check` logic uses different rules depending on whether we're using `uv pip` or `uv sync`.

---

_Comment by @yumeminami on 2025-08-23 04:15_

@charliermarsh 

Thanks your feedback! 

I took a look of #15450 and I think it did the thing like as discussed in https://github.com/astral-sh/uv/pull/15234#issuecomment-3182003235 by passing the `SyncModel`(`Sourcestrategy` in the comment) argument through multiple functions until it reaches `RequirementSatisfaction::check`.

I have some suggestions: 

1. `SyncModel` implies it's about `uv sync`? I'm not sure, but it actually controls the "packet matching strategy" and I feel like `Stateful` and `Stateless` might be a bit ambiguous. And I sugget use this `InstallationStrategy`, what do you think?

```rust
/// Defines the strategy for determining whether an installed package satisfies a requirement.
  ///
  /// This enum controls how the package installer validates existing installations against
  /// new requirements, particularly when there are differences in source types (registry vs local).
  #[derive(Debug, Clone, Copy, PartialEq, Eq)]
  pub enum InstallationStrategy {
      /// Permissive strategy: Accept existing installations even if source type differs.
      ///
      /// This strategy mirrors traditional pip behavior, where already-installed packages
      /// are reused if they satisfy the version requirements, regardless of how they were
      /// originally installed. For example:
      /// - If `./path/to/package` is installed and later `package` is requested from registry,
      ///   the existing installation will be reused if the version matches.
      /// - This enables incremental package management where users can mix installation sources.
      ///
      /// Used by: `uv pip install`, `uv pip sync`
      Permissive,

      /// Strict strategy: Require exact source type matching between installed and requested packages.
      ///
      /// This strategy enforces that the installation source must match the requirement source.
      /// It prevents reusing packages that were installed from different sources, ensuring
      /// declarative and reproducible environments. For example:
      /// - If an editable package is installed but a registry version is requested,
      ///   the editable installation will be replaced with the registry version.
      /// - This is essential for `--no-sources` scenarios where local installations
      ///   should be replaced with registry equivalents.
      ///
      /// Used by: `uv sync`, `uv run`, `uv tool run`
      Strict,
  }
```

2. Does this PR fix the issue in https://github.com/astral-sh/uv/pull/15234#issuecomment-3182044630 about `pip install --no-soures`? If the checks detect a `Statefule` condition and a `distribution mismatch` `InstalledDist::Registry { .. }`, should we prompt the user?

---

_Comment by @yumeminami on 2025-08-23 04:25_

I’ve attempted to add `SyncModel` as a property of the `SitePackages` struct so that we could update the construction of `SitePackages` instances instead of modifying multiple function signatures and calls.

And I found Rust does not support default parameters in functions—by default, `Stateful` is used, and `Stateless` must be explicitly passed when needed. This makes the solution more involved than the approach in [PR #15450](https://github.com/astral-sh/uv/pull/15450).

😢 😢 

---

_Comment by @charliermarsh on 2025-08-24 12:20_

Yeah I think your names are better (mine were sort of lazy, to validate the idea).

I think this should _only_ apply to `uv sync` personally, and even `uv pip install --no-sources` should have the same behavior whereby we "accept" the implicit registry requirement.

We might (?) need to thread this through to the candidate selector / resolver as well, is the only other downside... That's why some tests are failing, I think.


---

_Comment by @yumeminami on 2025-08-25 02:03_

Yeah, I agree.

If we want the `uv pip install --no-sources` has the same behave as `uv sycn --no-sources`, the candidate selector strategy need to align the `RequirementSatisfaction::check` func.

---

_Comment by @yumeminami on 2025-08-25 02:05_

@charliermarsh 

Later, I will base #15450 then modify the candidate selector strategy and new a pr again.(maybe Tuesday or Wednesday）

---

_Comment by @yumeminami on 2025-08-27 06:13_

@charliermarsh 

Now th PR should fix `uv sync --no-sources` and `uv pip install --no-sources`.

---

_Comment by @yumeminami on 2025-08-27 06:22_

how to rerun the CI and why the `install python` job will fail?

---

_Comment by @charliermarsh on 2025-09-14 02:05_

@yumeminami -- Sorry, I was trying to convey that I think this should _only_ affect `uv sync` (and `uv pip install` should be unchanged). Do you agree?

---

_Comment by @yumeminami on 2025-09-16 01:51_

Agree.

On Sun, 14 Sept 2025 at 10:06, Charlie Marsh ***@***.***>
wrote:

> *charliermarsh* left a comment (astral-sh/uv#15234)
> <https://github.com/astral-sh/uv/pull/15234#issuecomment-3289087527>
>
> @yumeminami <https://github.com/yumeminami> -- Sorry, I was trying to
> convey that I think this should *only* affect uv sync (and uv pip install
> should be unchanged). Do you agree?
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/pull/15234#issuecomment-3289087527>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AQDB34QOSDH6KYLOJZ6Q7EL3STER3AVCNFSM6AAAAACDV3WXSKVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTEOBZGA4DONJSG4>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @charliermarsh on 2025-09-16 01:52_

Okay thanks. I will revisit this and look to get it merged.

---

_Comment by @charliermarsh on 2025-09-17 01:45_

Okay, this should be good to go. It only changes the behavior for `uv sync` (and `uv run`, which calls `uv sync` behind the hood), where we have full control over the sources and resolution.

---

_Comment by @yumeminami on 2025-09-17 01:59_

@charliermarsh thanks!

---

_@zanieb approved on 2025-09-17 11:35_

---

_Merged by @zanieb on 2025-09-17 11:35_

---

_Closed by @zanieb on 2025-09-17 11:35_

---
