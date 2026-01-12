```yaml
number: 16157
title: Sync latest Python releases
type: pull_request
state: merged
author: github-actions
labels: []
assignees: []
merged: true
base: main
head: sync-python-releases
created_at: 2025-10-07T17:01:21Z
updated_at: 2025-10-07T20:19:37Z
url: https://github.com/astral-sh/uv/pull/16157
synced_at: 2026-01-12T16:12:08Z
```

# Sync latest Python releases

---

_@github-actions_

Automated update for Python releases.

---

_Closed by @geofft on 2025-10-07 17:05_

---

_Reopened by @geofft on 2025-10-07 17:05_

---

_@zanieb approved on 2025-10-07 18:34_

---

_Comment by @zanieb on 2025-10-07 18:34_

```
       FAIL [  27.530s] uv::it python_upgrade::python_upgrade
  stdout ───

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: python_upgrade-7
    Source: crates/uv/tests/it/python_upgrade.rs:79
    ────────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
        3     3 │ ----- stdout -----
        4     4 │ 
        5     5 │ ----- stderr -----
        6     6 │ warning: `uv python upgrade` is experimental and may change without warning. Pass `--preview-features python-upgrade` to disable this warning
        7       │-Installed Python 3.14.0rc3 in [TIME]
        8       │- + cpython-3.14.0rc3-[PLATFORM] (python3.14)
              7 │+Installed Python 3.14.0 in [TIME]
              8 │+ + cpython-3.14.0-[PLATFORM]
```

---

_Review comment by @geofft on `crates/uv/tests/it/python_install.rs`:3451 on 2025-10-07 19:07_

So this is interesting, it implies that installing 3.13 didn't update the python3.13 symlink and left it pointing to pyodide, which seems a bit semantically contradictory with the test that uv's preferred Python version is normal Python. (The updated test now installs 3.14 which is the target of the new python3.14 symlink.)

I can repro this with prod uv:
```
$ uv python uninstall 3.13
$ uv python install cpython-3.13.2-emscripten-wasm32-musl
Installed Python 3.13.2 in 1.12s
 + pyodide-3.13.2-emscripten-wasm32-musl (python3.13)
$ uv python install cpython-3.13
Installed Python 3.13.7 in 1.38s
 + cpython-3.13.7-linux-x86_64-gnu
$ uv python find cpython-3.13
/home/ubuntu/.local/share/uv/python/cpython-3.13.7-linux-x86_64-gnu/bin/python3.13
$ ls -l ~/.local/bin/python3.13
lrwxrwxrwx 1 ubuntu ubuntu 80 Oct  7 19:02 /home/ubuntu/.local/bin/python3.13 -> /home/ubuntu/.local/share/uv/python/pyodide-3.13.2-emscripten-wasm32-musl/python
```
i.e., uv prefers normal Python to satisfy a request for Python 3.13, but the bin symlink still points to pyodide.

Was this intentional? I don't think this is a release blocker but I can spin this out into a bug if we think this isn't the right behavior.

---

_Merged by @geofft on 2025-10-07 19:07_

---

_Closed by @geofft on 2025-10-07 19:07_

---

_Branch deleted on 2025-10-07 19:07_

---

_@geofft reviewed on 2025-10-07 20:19_

---
