```yaml
number: 11083
title: Avoid resolving symbolic links when querying Python interpreters
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-canonical
created_at: 2025-01-29T20:39:39Z
updated_at: 2025-01-30T16:10:36Z
url: https://github.com/astral-sh/uv/pull/11083
synced_at: 2026-01-12T16:09:40Z
```

# Avoid resolving symbolic links when querying Python interpreters

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/11048

This brings the `PythonEnvironment::from_root` behavior in-line with the rest of uv Python discovery behavior (and in-line with pip). It's not clear why we were canonicalizing the path in the first place here.


---

_Label `bug` added by @zanieb on 2025-01-29 20:39_

---

_Comment by @zanieb on 2025-01-29 20:50_

!!!

```
running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: run_linked_environment_path-2
Source: crates/uv/tests/it/run.rs:3395
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3       │-[VENV]/
    4       │-[VENV]/[BIN]/python
          3 │+[TEMP_DIR]/target
          4 │+[TEMP_DIR]/target/[BIN]/python
    5     5 │ 
    6     6 │ ----- stderr -----
    7     7 │ Resolved 8 packages in [TIME]
    8     8 │ Audited 6 packages in [TIME]
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test run::run_linked_environment_path ... FAILED
```

So.. the behavior is different on Linux than macOS?

---

_Comment by @zanieb on 2025-01-29 21:42_

Okay... so the test fails on Linux _and_ macOS in CI so the behavior is different in CI than on my machine... that's annoying.

---

_Comment by @zanieb on 2025-01-29 21:46_

Another development — if I use Python provided by Homebrew, the link is resolved (e.g., VENV not TARGET) but if I use a uv-managed Python, it is not.

---

_Comment by @zanieb on 2025-01-29 21:51_

We do have non-snapshot tests for HomeBrew Python and the existing behavior is _fine_ just not ideal, so I'm not too worried about shipping this regardless. It could be nice to understand _why_ this does not change anything in that case, and if any downstream packagers will be affected by this change — but I think the worst-case scenario is that they need to skip the test.

---

_Marked ready for review by @zanieb on 2025-01-29 21:51_

---

_Comment by @edmorley on 2025-01-29 22:25_

Thank you for looking into this!

> Another development — if I use Python provided by Homebrew, the link is resolved (e.g., VENV not TARGET) but if I use a uv-managed Python, it is not.

Reading the CPython implementation comments for executable/prefix/... calculation (since it's not covered in detail in the user-facing docs):
https://github.com/python/cpython/blob/a1a4e9f39ad86359e148fd193089b3b2a354f71d/Modules/getpath.py#L93-L153

...I see:

> Before any searches are done, the location of the executable is determined.  If Py_SetPath() was called, or if we are running on Windows, the 'real_executable' path is used (if known).  Otherwise, we use the config-specified program name or default to argv[0].

I'm not sure what "config-specified" means - could that be `_sysconfigdata__*` - and thus depend on whether the install has been relocated/whether that metadata references a real path and otherwise will be ignored? The CPython implementation after the comments linked above also has quite a few darwin-specific conditionals. (I only skimmed that file in a browser and it's 800 lines long so these are mostly only lazy guesses while I'm watching TV - sorry! 😆 )

---

_@charliermarsh approved on 2025-01-30 01:04_

There's no need to absolutize this, right? It doesn't get returned to the user in any way -- just the `sys.executable`?

---

_Comment by @zanieb on 2025-01-30 01:37_

I don't think so. The only change would be in logs, I think. I can do an audit though.

---

_Comment by @zanieb on 2025-01-30 16:10_

Oh, we return `root: interpreter.sys_prefix().to_path_buf(),` so.. yeah there's no need to absolutize the path.

---

_Merged by @zanieb on 2025-01-30 16:10_

---

_Closed by @zanieb on 2025-01-30 16:10_

---

_Branch deleted on 2025-01-30 16:10_

---
