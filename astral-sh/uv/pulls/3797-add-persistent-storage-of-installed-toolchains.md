```yaml
number: 3797
title: Add persistent storage of installed toolchains
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/data-dir-toolchain
created_at: 2024-05-23T16:40:38Z
updated_at: 2024-05-27T03:54:51Z
url: https://github.com/astral-sh/uv/pull/3797
synced_at: 2026-01-12T16:05:51Z
```

# Add persistent storage of installed toolchains

---

_@zanieb_

Extends #3726 

Moves toolchain storage out of `UV_BOOTSTRAP_DIR` (`./bin`) into the proper user data directory as defined by #3726.

Replaces `UV_BOOTSTRAP_DIR` with `UV_TOOLCHAIN_DIR` for customization. Installed toolchains will be discovered without opt-in, but the idea is still that these are not yet user-facing.

---

_Label `preview` added by @zanieb on 2024-05-23 16:40_

---

_Comment by @zanieb on 2024-05-23 21:22_

I'm going to write a `TestContext` for `uv-interpreter` to make the setup less complicated (#3832)

---

_Marked ready for review by @zanieb on 2024-05-23 21:22_

---

_Comment by @zanieb on 2024-05-23 21:23_

There's an open question about versioning. We probably shouldn't just discard old buckets like we do in the cache. Maybe we can version and write explicit migrations? 

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/managed/find.rs`:52 on 2024-05-24 00:39_

You may want to look at the cache initialization. We probably want to add a `.gitignore` and a `CACHEDIR.tag` to this?

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/managed/find.rs`:25 on 2024-05-24 00:40_

I've gone back and forth on `Into<PathBuf>`, it's sort of a hidden allocation if you pass in `&Path` I think? Not sure what's best.

---

_@charliermarsh approved on 2024-05-24 00:43_

---

_@charliermarsh reviewed on 2024-05-24 00:45_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/managed/find.rs`:143 on 2024-05-24 00:45_

You could do something like:

```diff
diff --git a/crates/uv-interpreter/src/managed/find.rs b/crates/uv-interpreter/src/managed/find.rs
index fd15c484..40f609cd 100644
--- a/crates/uv-interpreter/src/managed/find.rs
+++ b/crates/uv-interpreter/src/managed/find.rs
@@ -128,7 +128,11 @@ impl InstalledToolchains {
                     .path
                     .file_name()
                     .map(OsStr::to_string_lossy)
-                    .is_some_and(|filename| filename.starts_with(&format!("cpython-{version}")))
+                    .is_some_and(|filename| {
+                        filename
+                            .strip_prefix("cpython-")
+                            .is_some_and(|prefix| prefix.starts_with(&version.to_string()))
+                    })
             }))
     }
```

That would avoid allocation for anything that doesn't start with `cpython-`. But, IDK, perhaps this already gets optimized out anyway.

---

_@charliermarsh reviewed on 2024-05-24 00:45_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/managed/find.rs`:143 on 2024-05-24 00:45_

What you have now is more readable, I'd leave it as-is.

---

_@zanieb reviewed on 2024-05-24 04:19_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/managed/find.rs`:143 on 2024-05-24 04:19_

Probably going to parse the folder name into metadata _once_ when constructing the `Toolchain` in the future anyway

---

_@zanieb reviewed on 2024-05-24 15:18_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/managed/find.rs`:52 on 2024-05-24 15:18_

I did copy this from the cache init.

I think if we add a `CACHEDIR.tag` some tools may delete this data presuming it's not important, which seems... less than ideal.

I was thinking about adding a `.gitignore` but am scared of all the bugs we encountered with it in the cache directory. I'm not sure about the `.git` boundary file and such. Hm.

---

_@zanieb reviewed on 2024-05-24 15:19_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/managed/find.rs`:25 on 2024-05-24 15:19_

👍 just going to stick with consistency for now but yeah that makes sense as a concern

---

_Comment by @zanieb on 2024-05-24 15:20_

Updated to include #3726 which wouldn't pass CI otherwise since it was unused.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:147 on 2024-05-24 16:20_

:tada:

---

_@konstin approved on 2024-05-24 17:04_

---

_Merged by @charliermarsh on 2024-05-27 03:54_

---

_Closed by @charliermarsh on 2024-05-27 03:54_

---

_Branch deleted on 2024-05-27 03:54_

---
