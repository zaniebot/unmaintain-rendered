```yaml
number: 2637
title: Speed up pyc compilation
type: issue
state: open
author: hauntsaninja
labels:
  - performance
assignees: []
created_at: 2024-03-24T03:54:41Z
updated_at: 2025-09-02T13:11:24Z
url: https://github.com/astral-sh/uv/issues/2637
synced_at: 2026-01-12T15:58:39Z
```

# Speed up pyc compilation

---

_@hauntsaninja_

Thanks for implementing https://github.com/astral-sh/uv/issues/1788 , it's been great! 

Since currently pyc compilation is implemented as a pass over the entire env, it can be quite costly:
```
λ uv pip install pypyp --compile 
Resolved 1 package in 123ms
Downloaded 1 package in 68ms
Installed 1 package in 28ms
Bytecode compiled 58064 files in 53.81s
 + pypyp==1.2.0
```
In this venv, it's about 1000x longer than it takes to install without pyc compilation.

Note this very slow time is on macOS, it's much better on Linux machines I have access to (more like 10s). See https://github.com/astral-sh/uv/issues/2326#issuecomment-1987366097 for my laptop specs

To be clear, this is not a particularly pressing issue. The need to bytecode compile deltas is much lower than when building things from scratch. Nevertheless, ideally uv should be significantly faster than pip in all usage scenarios.

With that in mind, some possible suggestions:

1. It looks like uv currently forces recompilation https://github.com/astral-sh/uv/blob/1181aa9be40b7f99334f7efd15d5102653d8b38b/crates/uv-installer/src/pip_compileall.py#L50
I'm not sure why this is... maybe something to do with checked hash validation that compileall doesn't handle correctly? The script predates #2086 , so maybe there's something else going on
(update: I've merged this change)

2. We could only bytecode compile the newly installed packages

3. If uv no longer forces recompilation, you could move the invalidation / mtime logic into Rust, not sure how much that would help, but could conceivably save you from shelling to compileall

4. Something something copy on write for pyc

---

_Comment by @hauntsaninja on 2024-03-24 04:03_

It looks like pip forces recompilation too, and this goes back to pip v1.5 https://github.com/pypa/pip/commit/7ec49dc2fb0a9dbf522e79d87e2bc13d29a2556e#diff-9b28941fa609b0277432cba3444d4595da4ef80ca7c04e9024c75bb0a15ae830R163

Couldn't discern a reason for that, but since it's only doing it for the newly installed package I guess it doesn't hurt much

---

_Label `performance` added by @AlexWaygood on 2024-03-24 11:23_

---

_Comment by @charliermarsh on 2024-03-24 22:28_

Makes sense! I assume the highest-impact thing here would be to only recompile newly-installed packages.

---

_Comment by @hauntsaninja on 2024-03-24 23:08_

Yeah, probably. I just tested my first suggestion since it's easy:
```
diff --git a/crates/uv-installer/src/pip_compileall.py b/crates/uv-installer/src/pip_compileall.py
index 47e0242f..20bb3c6e 100644
--- a/crates/uv-installer/src/pip_compileall.py
+++ b/crates/uv-installer/src/pip_compileall.py
@@ -47,7 +47,7 @@ with warnings.catch_warnings():
         # We'd like to show those errors, but given that pip thinks that's totally fine,
         # we can't really change that.
         success = compileall.compile_file(
-            path, invalidation_mode=invalidation_mode, force=True, quiet=2
+            path, invalidation_mode=invalidation_mode, force=False, quiet=2
         )
         # We're ready for the next file.
         print(path)
```
And it speeds things up 2-3x, in the above case from 55s to 20s. Would probably stack well with the third suggestion too, in case we like compiling the whole venv (if it's fast no reason not to)

---

_Comment by @charliermarsh on 2024-03-24 23:13_

I'm cool with shipping that.

---

_Comment by @tmke8 on 2025-09-02 08:32_

This issue has become newly relevant because of [PEP 765](https://peps.python.org/pep-0765/)'s inclusion in Python 3.14. Without precompilation, users will see the syntax warning even from third-party code (on the first run at least), which might break CIs and might be confusing. If precompilation were faster, it could maybe be enabled by default in uv (as it is in pip) and avoid these problems stemming from PEP 765.

---

_Comment by @notatallshaw on 2025-09-02 12:39_

@tmke8 this issue is about speeding up the byte code compilation. You should make a new issue if you think uv should behave differently because of PEP 765 (I don't but that should be discussed in it's own issue).

Edit: While I understand you're making an argument based on if things were faster, I think if using compilation should be used to hide syntax warnings by default should be evaluated on it's own merit. 

---

_Comment by @konstin on 2025-09-02 12:43_

For context, there's some prior discussion on this: https://github.com/astral-sh/uv/issues/1928

---
