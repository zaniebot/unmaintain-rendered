```yaml
number: 2900
title: "uv panics when trying to create more than one RequirementsSource from a file without `-r`"
type: issue
state: closed
author: slafs
labels:
  - bug
assignees: []
created_at: 2024-04-08T12:06:55Z
updated_at: 2024-04-08T17:18:55Z
url: https://github.com/astral-sh/uv/issues/2900
synced_at: 2026-01-10T05:40:32Z
```

# uv panics when trying to create more than one RequirementsSource from a file without `-r`

---

_Issue opened by @slafs on 2024-04-08 12:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

uv panics when trying to create more than one `RequirementsSource` instance in the `from_package` method when the given name is a file.

The below steps (ran inside `python:3.11-alpine` container, but the same happens on OSX 14.4.1) will reproduce the crash:
```sh
# pip install uv
Collecting uv
  Downloading uv-0.1.29-py3-none-musllinux_1_2_x86_64.whl.metadata (26 kB)
Downloading uv-0.1.29-py3-none-musllinux_1_2_x86_64.whl (11.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 11.5/11.5 MB 8.1 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.1.29
# echo cowsay > ./req1.txt
# echo cowsay > ./req2.txt
# uv pip install req1.txt req2.txt
✔ `req1.txt` looks like a local requirements file but was passed as a package name. Did you mean `-r req1.txt`? · yes
thread 'main' panicked at crates/uv-requirements/src/sources.rs:98:75:
called `Result::unwrap()` on an `Err` value: Ctrl-C error: Ctrl-C signal handler already registered
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This happens when trying to prompt the user to auto convert `pip install file.txt` into a proper `-r` form (which is a very nice feature, that I'm planning to utilize often BTW 👍).

With one file everything's fine, but when passing more than one (e.g. with a glob),
uv panics, because it tries to unwrap on an error from the `ctrlc::set_handler` call.
The error apparently happens when trying to set more than one handler for <kbd>Ctrl</kbd>+<kbd>C</kbd>.

This workaround works for me:

```diff
diff --git a/crates/uv-requirements/src/confirm.rs b/crates/uv-requirements/src/confirm.rs
index 3b111d33..e8737b73 100644
--- a/crates/uv-requirements/src/confirm.rs
+++ b/crates/uv-requirements/src/confirm.rs
@@ -6,7 +6,7 @@ use console::{style, Key, Term};
 /// This is a slimmed-down version of `dialoguer::Confirm`, with the post-confirmation report
 /// enabled.
 pub(crate) fn confirm(message: &str, term: &Term, default: bool) -> Result<bool> {
-    ctrlc::set_handler(move || {
+    let handler_set = ctrlc::set_handler(move || {
         let term = Term::stderr();
         term.show_cursor().ok();
         term.flush().ok();
@@ -17,7 +17,13 @@ pub(crate) fn confirm(message: &str, term: &Term, default: bool) -> Result<bool>
         } else {
             130
         });
-    })?;
+    });
+
+    // silence MultipleHandlers error from ctrlc
+    match handler_set {
+        Ok(_) | Err(ctrlc::Error::MultipleHandlers) => {},
+        Err(e) => return Err(e.into()),
+    }
 
     let prompt = format!(
         "{} {} {} {} {}",
```

Which I would be more than happy to turn into a PR 🤞.

However, looking at [the `set_handler` docs from `ctrlc`](http://detegr.github.io/doc/ctrlc/fn.set_handler.html) it looks like it shouldn't ever return MultipleHandlers (there's a `try_set_handler` function for that) and it should overwrite the existing handler 🤷.

I've asked this question in https://github.com/Detegr/rust-ctrlc/issues/118

---

_Label `bug` added by @charliermarsh on 2024-04-08 13:29_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-08 13:29_

---

_Comment by @charliermarsh on 2024-04-08 13:29_

Thanks!

---

_Unassigned @charliermarsh by @charliermarsh on 2024-04-08 13:29_

---

_Comment by @charliermarsh on 2024-04-08 13:42_

My read is that `set_handler` _will_ return an error if called twice. But if there's an existing handler set by the system, it will overwrite it. So I think we should proceed with your change. PR welcome.

---

_Closed by @charliermarsh on 2024-04-08 17:18_

---
