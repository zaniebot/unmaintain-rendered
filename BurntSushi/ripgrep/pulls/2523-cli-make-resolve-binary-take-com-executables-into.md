```yaml
number: 2523
title: "cli: make `resolve_binary()` take COM executables into account"
type: pull_request
state: closed
author: mataha
labels:
  - rollup
assignees: []
base: master
head: fix/cli/resolve-com-binaries
created_at: 2023-05-30T00:24:29Z
updated_at: 2023-07-18T17:26:49Z
url: https://github.com/BurntSushi/ripgrep/pull/2523
synced_at: 2026-01-12T18:23:14Z
```

# cli: make `resolve_binary()` take COM executables into account

---

_@mataha_

When `resolve_binary()` attempts to resolve a path to a program on Windows while searching for a program in `PATH` without an extension, `ripgrep` will assume the extension of the file to be `.exe` as it's the *de facto* standard, which will work most (99.99%) of the time...

...unless the binary is a COM executable (we're on Windows, duh).

(It feels weird being the first person to run into this.)

Related: sharkdp/bat#2570 (`more` is a COM executable)

---

_@mataha reviewed on 2023-05-30 11:55_

---

_Review comment by @mataha on `crates/cli/src/decompress.rs`:458 on 2023-05-30 11:55_

I've added `com` here, but frankly I don't think that's the way to go - it probably would be better to respect `PATHEXT` in terms of executable extensions.

Opinion?

---

_@BurntSushi approved on 2023-07-05 18:53_

---

_@BurntSushi reviewed on 2023-07-05 18:54_

---

_Review comment by @BurntSushi on `crates/cli/src/decompress.rs`:458 on 2023-07-05 18:54_

I don't think Rust has a `PATHEXT` for Windows? The closest thing it has is [`std::env::consts::EXE_SUFFIX`](https://doc.rust-lang.org/std/env/consts/constant.EXE_SUFFIX.html) (or [`std::env::consts::EXE_EXTENSION`](https://doc.rust-lang.org/std/env/consts/constant.EXE_EXTENSION.html)), but it's just a single value, not a list.

---

_Label `rollup` added by @BurntSushi on 2023-07-05 18:56_

---

_Review comment by @mataha on `crates/cli/src/decompress.rs`:458 on 2023-07-06 15:41_

Rust doesn't have it indeed - it needs to be pulled manually.

This is the best I came up with; `PATHEXT` entries contain leading dots so these have to be trimmed:

```diff
diff --git a/crates/cli/src/decompress.rs b/crates/cli/src/decompress.rs
index f2f2a67478ca..4fc257bb7603 100644
--- a/crates/cli/src/decompress.rs
+++ b/crates/cli/src/decompress.rs
@@ -455,8 +455,14 @@ pub fn resolve_binary<P: AsRef<Path>>(
             return Ok(abs_prog.to_path_buf());
         }
         if abs_prog.extension().is_none() {
-            for extension in ["com", "exe"] {
-                let abs_prog = abs_prog.with_extension(extension);
+            let exts = match env::var_os("PATHEXT") {
+                Some(exts) => exts,
+                None => env::consts::EXE_SUFFIX.into(),
+            };
+            for ext in env::split_paths(&exts)
+                .filter_map(|e| e.to_str().map(|s| s.to_lowercase()))
+            {
+                let abs_prog = abs_prog.with_extension(ext.trim_matches('.'));
                 if is_exe(&abs_prog) {
                     return Ok(abs_prog.to_path_buf());
                 }
```

(sorry for the double message - I've accidentally clicked the wrong button)

---

_@mataha reviewed on 2023-07-06 15:41_

---

_@BurntSushi reviewed on 2023-07-06 15:44_

---

_Review comment by @BurntSushi on `crates/cli/src/decompress.rs`:458 on 2023-07-06 15:44_

I'm just going to go with hard-coding `com` and `exe` for now. If that's not enough, we can revisit it if someone runs into a real problem.

---

_Review comment by @mataha on `crates/cli/src/decompress.rs`:458 on 2023-07-06 15:45_

Sure!

---

_@mataha reviewed on 2023-07-06 15:45_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-07-10 21:56_

---

_Comment by @mataha on 2023-07-18 17:19_

Is there a chance for `grep-cli` to receive a release with this fix included?

---

_Comment by @BurntSushi on 2023-07-18 17:25_

This PR is on crates.io in `grep-cli 0.1.9`.

---

_Comment by @mataha on 2023-07-18 17:26_

Thanks!

---
