```yaml
number: 16853
title: Allow discovery of venv in VIRTUAL_ENV env variable
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: discovery-activated-venv
created_at: 2025-03-19T17:30:35Z
updated_at: 2025-03-20T14:00:41Z
url: https://github.com/astral-sh/ruff/pull/16853
synced_at: 2026-01-10T19:40:36Z
```

# Allow discovery of venv in VIRTUAL_ENV env variable

---

_Pull request opened by @MatthewMckee4 on 2025-03-19 17:30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes astral-sh/ty#171 

Allows the cli to find a virtual environment from the VIRTUAL_ENV environment variable if no --python is set

## Test Plan

Unsure how to mock a venv to test that this works

---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 17:35_

---

_Comment by @github-actions[bot] on 2025-03-19 17:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:166 on 2025-03-19 17:41_

Using this approach for when `VIRTUAL_ENV` is set does make sense to me because we probably want Red Knot to fail if *whatever `VIRTUAL_ENV`* is pointing to doesn't turn out to be a virtual env. 

I'm not sure if it's the right approach to e.g. support `.venv` because we then only want to use the folder if it actually is a virtual env.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:148 on 2025-03-19 17:41_

What's the motivation for introducing a new variant over using `SysPrefix`?

---

_@MichaReiser reviewed on 2025-03-19 17:41_

Nice

---

_Comment by @github-actions[bot] on 2025-03-19 18:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb reviewed on 2025-03-19 19:57_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/program.rs`:166 on 2025-03-19 19:57_

I should probably look at this for parity with uv..

---

_Marked ready for review by @MatthewMckee4 on 2025-03-20 11:23_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-20 11:23_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-20 11:23_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-20 11:23_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-20 11:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/src/metadata/options.rs`:111 on 2025-03-20 11:29_

Arguments to `.or()` are eagerly evaluated. If we use `.or_else()`, it will be lazily evaluated:

```suggestion
                .or_else(|| PythonPath::find_virtual_env(system))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:231 on 2025-03-20 11:31_

I think you can just use a reference here rather than a clone -- it should be slightly more efficient:

```suggestion
                VirtualEnvironment::new(&sys_prefix.inner, sys_prefix.origin, system)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:4 on 2025-03-20 11:32_

nit:

```suggestion
use crate::site_packages::{SysPrefixPath, SysPrefixPathOrigin};
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:165 on 2025-03-20 11:36_

You can simplify this a little by short-circuiting immediately using `?` if `std::env::var("VIRTUAL_ENV").ok()` returns `None`:

```suggestion
        let virtual_env = std::env::var("VIRTUAL_ENV").ok()?;
        tracing::debug!("Found virtual environment at {:?}", virtual_env);
        SysPrefixPath::new(virtual_env, SysPrefixPathOrigin::VirtualEnvVar, system)
            .ok()
            .map(Self::SysPrefix)
```

I think it might also be good to say in the tracing message that we found the virtual environment from the `VIRTUAL_ENV` variable rather than `--python` or anything else -- that could be useful information when debugging. So maybe something like this?

```suggestion
        let virtual_env = std::env::var("VIRTUAL_ENV").ok()?;
        tracing::debug!("Found virtual environment at {:?} from `VIRTUAL_ENV` environment variable", virtual_env);
        SysPrefixPath::new(virtual_env, SysPrefixPathOrigin::VirtualEnvVar, system)
            .ok()
            .map(Self::SysPrefix)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:385 on 2025-03-20 11:39_

it might be slightly better to keep these fields private and have `pub(crate)` accessor methods:

```suggestion
pub struct SysPrefixPath {
    inner: SystemPathBuf,
    origin: SysPrefixPathOrigin,
}

impl SysPrefixPath {
    pub(crate) fn inner(&self) -> &SystemPath {
        &self.inner
    }
    
    pub(crate) fn origin(&self) -> SysPrefixPathOrigin {
        self.origin
    }

```

though it probably doesn't matter that much in practice!

---

_@AlexWaygood reviewed on 2025-03-20 11:40_

this is looking great!!

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-20 12:10_

---

_@AlexWaygood reviewed on 2025-03-20 12:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:62 on 2025-03-20 12:26_

I've realised that there's an issue here, which is that now the path to the virtual environment is validated twice:
- Once eagerly in `PythonPath::find_virtual_env()` / `PythonPath::from_cli_flag()`
- And then again later on in `VirtualEnvironment::new()` here

It's better if we validate the path to the virtual environment later, because we can use the `Result` return type of `VirtualEnvironment::new()` to give a nice error message to the user about why the path given to the virtual environment is invalid (with the eager validation that we're doing on this PR branch currently, we have to throw away all the details about the error by calling `.ok()` on it). The validation is done by `SysPrefixPath::new()`, so if we only want the validation to be done inside this module, that implies that we should make `SysPrefixPath` private again. Instead, I think we can just add another field to the `SysPrefix` variant of `PythonPath`. Something like this (the diff is relative to your branch):

```diff
diff --git a/crates/red_knot_project/src/metadata/options.rs b/crates/red_knot_project/src/metadata/options.rs
index b115141ee..43a6c4233 100644
--- a/crates/red_knot_project/src/metadata/options.rs
+++ b/crates/red_knot_project/src/metadata/options.rs
@@ -105,10 +105,14 @@ impl Options {
             src_roots,
             custom_typeshed: typeshed.map(|path| path.absolute(project_root, system)),
             python_path: python
-                .and_then(|python_path| {
-                    PythonPath::from_cli_flag(python_path.absolute(project_root, system), system)
+                .map(|python_path| {
+                    PythonPath::from_cli_flag(python_path.absolute(project_root, system))
+                })
+                .or_else(|| {
+                    std::env::var("VIRTUAL_ENV")
+                        .ok()
+                        .map(PythonPath::from_virtual_env_var)
                 })
-                .or_else(|| PythonPath::find_virtual_env(system))
                 .unwrap_or_else(|| PythonPath::KnownSitePackages(vec![])),
         }
     }
diff --git a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
index 1ce5c3ea2..ba9150f3c 100644
--- a/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
+++ b/crates/red_knot_python_semantic/src/module_resolver/resolver.rs
@@ -223,12 +223,12 @@ impl SearchPaths {
         static_paths.push(stdlib_path);
 
         let site_packages_paths = match python_path {
-            PythonPath::SysPrefix(sys_prefix) => {
+            PythonPath::SysPrefix(sys_prefix, origin) => {
                 // TODO: We may want to warn here if the venv's python version is older
                 //  than the one resolved in the program settings because it indicates
                 //  that the `target-version` is incorrectly configured or that the
                 //  venv is out of date.
-                VirtualEnvironment::new(sys_prefix.inner(), sys_prefix.origin(), system)
+                VirtualEnvironment::new(sys_prefix, *origin, system)
                     .and_then(|venv| venv.site_packages_directories(system))?
             }
 
diff --git a/crates/red_knot_python_semantic/src/program.rs b/crates/red_knot_python_semantic/src/program.rs
index 5315be124..325d594fa 100644
--- a/crates/red_knot_python_semantic/src/program.rs
+++ b/crates/red_knot_python_semantic/src/program.rs
@@ -1,10 +1,10 @@
 use crate::module_resolver::SearchPaths;
 use crate::python_platform::PythonPlatform;
-use crate::site_packages::{SysPrefixPath, SysPrefixPathOrigin};
+use crate::site_packages::SysPrefixPathOrigin;
 use crate::Db;
 
 use anyhow::Context;
-use ruff_db::system::{System, SystemPath, SystemPathBuf};
+use ruff_db::system::{SystemPath, SystemPathBuf};
 use ruff_python_ast::PythonVersion;
 use salsa::Durability;
 use salsa::Setter;
@@ -143,7 +143,7 @@ pub enum PythonPath {
     /// `/opt/homebrew/lib/python3.X/site-packages`.
     ///
     /// [`sys.prefix`]: https://docs.python.org/3/library/sys.html#sys.prefix
-    SysPrefix(SysPrefixPath),
+    SysPrefix(SystemPathBuf, SysPrefixPathOrigin),
 
     /// Resolved site packages paths.
     ///
@@ -153,20 +153,11 @@ pub enum PythonPath {
 }
 
 impl PythonPath {
-    pub fn find_virtual_env(system: &dyn System) -> Option<Self> {
-        let virtual_env = std::env::var("VIRTUAL_ENV").ok()?;
-        tracing::debug!(
-            "Found virtual environment at {:?} from `VIRTUAL_ENV` environment variable",
-            virtual_env
-        );
-        SysPrefixPath::new(virtual_env, SysPrefixPathOrigin::VirtualEnvVar, system)
-            .ok()
-            .map(Self::SysPrefix)
+    pub fn from_virtual_env_var(path: impl Into<SystemPathBuf>) -> Self {
+        Self::SysPrefix(path.into(), SysPrefixPathOrigin::VirtualEnvVar)
     }
 
-    pub fn from_cli_flag(path: SystemPathBuf, system: &dyn System) -> Option<Self> {
-        SysPrefixPath::new(path, SysPrefixPathOrigin::PythonCliFlag, system)
-            .ok()
-            .map(Self::SysPrefix)
+    pub fn from_cli_flag(path: SystemPathBuf) -> Self {
+        Self::SysPrefix(path, SysPrefixPathOrigin::PythonCliFlag)
     }
 }
diff --git a/crates/red_knot_python_semantic/src/site_packages.rs b/crates/red_knot_python_semantic/src/site_packages.rs
index 99f8837db..0d97a5945 100644
--- a/crates/red_knot_python_semantic/src/site_packages.rs
+++ b/crates/red_knot_python_semantic/src/site_packages.rs
@@ -377,20 +377,12 @@ fn site_packages_directory_from_sys_prefix(
 /// [`sys.prefix`]: https://docs.python.org/3/library/sys.html#sys.prefix
 #[derive(Debug, PartialEq, Eq, Clone)]
 #[cfg_attr(feature = "serde", derive(serde::Serialize, serde::Deserialize))]
-pub struct SysPrefixPath {
+pub(crate) struct SysPrefixPath {
     inner: SystemPathBuf,
     origin: SysPrefixPathOrigin,
 }
 
 impl SysPrefixPath {
-    pub(crate) fn inner(&self) -> &SystemPath {
-        &self.inner
-    }
-
-    pub(crate) fn origin(&self) -> SysPrefixPathOrigin {
-        self.origin
-    }
-
     pub(crate) fn new(
         unvalidated_path: impl AsRef<SystemPath>,
         origin: SysPrefixPathOrigin,
@@ -464,7 +456,7 @@ impl fmt::Display for SysPrefixPath {
 
 #[derive(Debug, PartialEq, Eq, Copy, Clone)]
 #[cfg_attr(feature = "serde", derive(serde::Serialize, serde::Deserialize))]
-pub(crate) enum SysPrefixPathOrigin {
+pub enum SysPrefixPathOrigin {
     PythonCliFlag,
     VirtualEnvVar,
     Derived,
```

---

_@zanieb reviewed on 2025-03-20 13:09_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/program.rs`:166 on 2025-03-20 13:09_

(there's actually not much to review here — uv's discovery is far more complicated and doesn't really seem relevant for this particular change)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/site_packages.rs`:48 on 2025-03-20 13:21_

Should we pass the `sys_prefix` path instead of `path` and `SysPrefixPathOrigin`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/site_packages.rs`:215 on 2025-03-20 13:22_

Same here, Should we just store the `SysPrefixPath` instead of individual fields or do we use separate fields to derive the error message?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:146 on 2025-03-20 13:24_

Same here. Should this store a `SysPrefixPath` instead of the two individual fields

---

_@MichaReiser approved on 2025-03-20 13:26_

Nice! 

I think we can use `SysPrefixPath` in more places to avoid having to reduce the number of arguments that need to be routed through the different levels.

---

_@AlexWaygood reviewed on 2025-03-20 13:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:146 on 2025-03-20 13:28_

`SysPrefixPath` represents a _validated_ path to `sys.prefix`, but at this stage the path hasn't yet been validated (and I don't think we want to do the validation this early; it's better if we do it in `SearchPaths::from_settings()`)

---

_@AlexWaygood reviewed on 2025-03-20 13:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:215 on 2025-03-20 13:34_

We can't store a `SysPrefixPath` here because the first two error variants are returned by `SysPrefixPath::new()`, so we don't have a validated `SysPrefixPath` yet at the point when we'd construct this error variant

---

_@MichaReiser reviewed on 2025-03-20 13:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/program.rs`:146 on 2025-03-20 13:36_

I see. It's somewhat annoying that we have to pass through all those arguments and I'm not sure if it's necessary to restrict `SysPrefixPath` to *valid paths* only. But changing this constraint seems out of the scope for this PR

---

_@AlexWaygood reviewed on 2025-03-20 13:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/program.rs`:146 on 2025-03-20 13:37_

yeah, agreed, we could consider a redesign where the validation is moved out of `SysPrefixPath::new()`. But I also agree that it's out of scope here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:379 on 2025-03-20 13:38_

I think this change is no longer needed on the latest version of the PR

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:386 on 2025-03-20 13:39_

same here

```suggestion
    fn new(
```

---

_@AlexWaygood reviewed on 2025-03-20 13:39_

---

_Comment by @MatthewMckee4 on 2025-03-20 13:39_

Could anyone help with testing, I'm unsure about how to go about testing this 

---

_Comment by @AlexWaygood on 2025-03-20 13:42_

> Could anyone help with testing, I'm unsure about how to go about testing this

I'm curious if @MichaReiser has any ideas but my best idea here is lots of manual testing:
- If we manually create an environment and activate it, can red-knot automatically resolve imports that refer to third-party packages in the venv's `site_packages` directory?
- If the environment is implicitly activated via `uv run`, can can red-knot automatically resolve imports that refer to third-party packages in the venv's `site_packages` directory?
- If we deliberately "break" a venv in one of the ways that we try to detect using the `site_packages::SitePackagesDiscoveryError` enum and then activate the venv and run red-knot, what do the error messages from red-knot look like?

---

_Comment by @MichaReiser on 2025-03-20 13:46_

An automated CLI test would be great, but it's probably somewhat painful to set up because `SysPrefixPath` does a lot of validation, and the behavior is even platform-dependent. I'm fine skipping an automated test here because it doesn't do much more than `--python <path>`

---

_@AlexWaygood reviewed on 2025-03-20 13:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/site_packages.rs`:218 on 2025-03-20 13:47_

I think we should get rid of the colon here. I manually tested this by:
1. Creating a virtual environment using `uv venv`
2. Breaking the virtual environment by running `rm .venv/pyvenv.cfg`
3. Activating the virtual environment by running `source .venv/bin/activate`
4. Running `../ruff/target/debug/red_knot check`

The error message displayed to the terminal was:

```
~/dev/typeshed-stats (main) % uv run ../ruff/target/debug/red_knot check
Red Knot failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: `VIRTUAL_ENV` environment variable: points to a broken venv with no pyvenv.cfg file
```

And I don't think the second colon on the last line there is necessary

```suggestion
    #[error("{0} points to a broken venv with no pyvenv.cfg file")]
```

---

_@AlexWaygood approved on 2025-03-20 13:48_

I manually tested with both "good" and "broken" virtual environments, and it all looks great. Thank you!

---

_Merged by @AlexWaygood on 2025-03-20 13:55_

---

_Closed by @AlexWaygood on 2025-03-20 13:55_

---

_Branch deleted on 2025-03-20 14:00_

---
