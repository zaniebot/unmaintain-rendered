```yaml
number: 4674
title: "feat: add tool version to list command"
type: pull_request
state: merged
author: caiquejjx
labels: []
assignees: []
merged: true
base: main
head: feat/add-tool-version-to-listing
created_at: 2024-07-01T00:12:07Z
updated_at: 2024-07-03T18:24:38Z
url: https://github.com/astral-sh/uv/pull/4674
synced_at: 2026-01-12T16:06:23Z
```

# feat: add tool version to list command

---

_@caiquejjx_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #4653

## Summary
Adds the tool version to the list command right beside the tool name

```
$ uv tool list
black v24.2.0
```

Following the proposed format discussed in #4653


## Test Plan
`cargo test tool_list`

<!-- How was it tested? -->


---

_Assigned to @zanieb by @zanieb on 2024-07-01 17:43_

---

_Comment by @zanieb on 2024-07-01 20:42_

Thanks for the pull request!

I'm not sure we should store the version in the receipt like this — can we instead read it from the site packages of the tool's virtual environment? I'm not entirely sure what the trade-offs would be here, but it'd be nice to avoid storing things in the receipt that are available elsewhere.

Let me know if you have any questions.


---

_Comment by @caiquejjx on 2024-07-01 21:27_

sure thing @zanieb makes sense, I was taking a look and if I understood correctly to do the way you mentioned I would need to instance an `Interpreter` to get an `PythonEnvironment` and from that get the `SitePackages`, is that right?
Similar to what is done in the install [step](https://github.com/astral-sh/uv/blob/61014d48b08b9f727255efe5d3db5612b8e613b3/crates/uv/src/commands/tool/install.rs#L123) 

---

_Comment by @caiquejjx on 2024-07-01 22:07_

@zanieb I have added a draft implementation of what I was able to come up with (quite verbose xD), can you take a look?

---

_Comment by @zanieb on 2024-07-01 22:09_

You can use `PythonEnvironment::from_root` as we do at https://github.com/astral-sh/uv/blob/324e9fe5cf67d98495427ceb9a390c854e2f9f18/crates/uv-tool/src/lib.rs#L179 then yeah you can grab the package information from site-packages as we do during `install` https://github.com/astral-sh/uv/blob/305868cdcc65aa7bc50378eb2cf65200a5a7a359/crates/uv/src/commands/tool/install.rs#L158-L162

---

_Comment by @zanieb on 2024-07-01 22:10_

Ah yeah that looks pretty reasonable but yeah kind of verbose let me see...

---

_Comment by @zanieb on 2024-07-01 22:25_

Yeah I took a swing at this and ended up with a pretty verbose implementation too

```diff
diff --git a/Cargo.lock b/Cargo.lock
index d846cc93..4df5f5d4 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -5048,6 +5048,7 @@ dependencies = [
  "tracing",
  "uv-cache",
  "uv-fs",
+ "uv-installer",
  "uv-state",
  "uv-toolchain",
  "uv-virtualenv",
diff --git a/crates/uv-tool/Cargo.toml b/crates/uv-tool/Cargo.toml
index 33426cab..3747e1e2 100644
--- a/crates/uv-tool/Cargo.toml
+++ b/crates/uv-tool/Cargo.toml
@@ -19,6 +19,7 @@ pep508_rs = { workspace = true }
 pypi-types = { workspace = true }
 uv-cache = { workspace = true }
 uv-fs = { workspace = true }
+uv-installer = { workspace = true }
 uv-state = { workspace = true }
 uv-toolchain = { workspace = true }
 uv-virtualenv = { workspace = true }
diff --git a/crates/uv-tool/src/lib.rs b/crates/uv-tool/src/lib.rs
index ecab0edf..da3db376 100644
--- a/crates/uv-tool/src/lib.rs
+++ b/crates/uv-tool/src/lib.rs
@@ -1,6 +1,7 @@
 use core::fmt;
 use std::io::{self, Write};
 use std::path::{Path, PathBuf};
+use std::str::FromStr;
 
 use fs_err as fs;
 use fs_err::File;
@@ -9,11 +10,12 @@ use tracing::debug;
 
 use install_wheel_rs::read_record_file;
 use pep440_rs::Version;
-use pep508_rs::PackageName;
+use pep508_rs::{InvalidNameError, PackageName};
 pub use receipt::ToolReceipt;
 pub use tool::{Tool, ToolEntrypoint};
 use uv_cache::Cache;
 use uv_fs::{LockedFile, Simplified};
+use uv_installer::SitePackages;
 use uv_state::{StateBucket, StateStore};
 use uv_toolchain::{Interpreter, PythonEnvironment};
 use uv_warnings::warn_user_once;
@@ -31,6 +33,10 @@ pub enum Error {
     ReceiptRead(PathBuf, #[source] Box<toml::de::Error>),
     #[error(transparent)]
     VirtualEnvError(#[from] uv_virtualenv::Error),
+    #[error("Failed to read tool environment packages at `{0}`: {1}")]
+    EnvironmentRead(PathBuf, String),
+    #[error("Failed find tool package `{0}` at `{1}`")]
+    MissingToolPackage(PackageName, PathBuf),
     #[error("Failed to read package entry points {0}")]
     EntrypointRead(#[from] install_wheel_rs::Error),
     #[error("Failed to find dist-info directory `{0}` in environment at {1}")]
@@ -38,6 +44,8 @@ pub enum Error {
     #[error("Failed to find a directory for executables")]
     NoExecutableDirectory,
     #[error(transparent)]
+    ToolName(#[from] InvalidNameError),
+    #[error(transparent)]
     EnvironmentError(#[from] uv_toolchain::Error),
     #[error("Failed to find a receipt for tool `{0}` at {1}")]
     MissingToolReceipt(String, PathBuf),
@@ -203,6 +211,19 @@ impl InstalledTools {
         ))
     }
 
+    pub fn version(&self, name: &str, cache: &Cache) -> Result<Version, Error> {
+        let environment_path = self.root.join(name);
+        let package_name = PackageName::from_str(name)?;
+        let environment = PythonEnvironment::from_root(&environment_path, cache)?;
+        let site_packages = SitePackages::from_environment(&environment)
+            .map_err(|err| Error::EnvironmentRead(environment_path.clone(), err.to_string()))?;
+        let packages = site_packages.get_packages(&package_name);
+        let package = packages
+            .first()
+            .ok_or_else(|| Error::MissingToolPackage(package_name, environment_path))?;
+        Ok(package.version().clone())
+    }
+
     /// Initialize the tools directory.
     ///
     /// Ensures the directory is created.
```

I just moved it into the `InstalledTools` type and added some error variants. I'm not sure we can do much better here?

---

_Comment by @zanieb on 2024-07-01 22:31_

As an aside, it probably makes sense for `uv tool list` to emit warnings when the environment for one of the tools is broken rather than a hard failure.

---

_Comment by @caiquejjx on 2024-07-01 22:47_

yeah I think there's not much to do to improve it besides what u already did, I think I'll start from where you stopped and handle the warnings/avoid hard failures, wdyt?

---

_Comment by @zanieb on 2024-07-02 01:29_

That sounds good to me!

---

_@zanieb reviewed on 2024-07-02 21:43_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:114 on 2024-07-02 21:43_

Minor note that there's no real preference between the two of these in our test suite. `unwrap()` is nice because you get a line number on failures. `?` is nice because it's succinct.

---

_Comment by @caiquejjx on 2024-07-02 22:07_

@zanieb can you help me understand why my test fails for windows?

---

_Comment by @zanieb on 2024-07-02 22:25_

@caiquejjx I think on Windows it's a `Scripts` directory not `bin`. You can use `virtualenv_python_executable` to find the path to the executable you want to remove in a cross-platform fashion.

---

_@zanieb reviewed on 2024-07-03 16:08_

---

_Review comment by @zanieb on `crates/uv/tests/tool_list.rs`:130 on 2024-07-03 16:08_

Well now we fixed deletion but we're still going to snapshot the wrong path :) Is there a different way to break the environment?

---

_@caiquejjx reviewed on 2024-07-03 16:49_

---

_Review comment by @caiquejjx on `crates/uv/tests/tool_list.rs`:130 on 2024-07-03 16:49_

I'm actually trying to add a filter for the path but it's being kinda tricky

---

_@caiquejjx reviewed on 2024-07-03 17:17_

---

_Review comment by @caiquejjx on `crates/uv/tests/tool_list.rs`:130 on 2024-07-03 17:17_

it worked

---

_@zanieb approved on 2024-07-03 17:54_

Thank you!

---

_Comment by @zanieb on 2024-07-03 18:17_

I broke it! I'll explore my filter change separately.

---

_Merged by @zanieb on 2024-07-03 18:24_

---

_Closed by @zanieb on 2024-07-03 18:24_

---
