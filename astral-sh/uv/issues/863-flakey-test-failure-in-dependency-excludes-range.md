```yaml
number: 863
title: "Flakey test failure in `dependency_excludes_range_of_compatible_versions`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-01-10T00:10:29Z
updated_at: 2024-01-28T14:27:23Z
url: https://github.com/astral-sh/uv/issues/863
synced_at: 2026-01-12T15:58:24Z
```

# Flakey test failure in `dependency_excludes_range_of_compatible_versions`

---

_@charliermarsh_

This test failed for me in https://github.com/astral-sh/puffin/actions/runs/7468310714/job/20323498659?pr=842:

```
---- dependency_excludes_range_of_compatible_versions stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: dependency_excludes_range_of_compatible_versions
Source: crates/puffin-cli/tests/pip_install_scenarios.rs:243
────────────────────────────────────────────────────────────────────────────────
program: puffin
args:
  - pip-install
  - dependency-excludes-range-of-compatible-versions-2023222f-a
  - "dependency-excludes-range-of-compatible-versions-2023222f-b>=2.0.0,<3.0.0"
  - dependency-excludes-range-of-compatible-versions-2023222f-c
  - "--extra-index-url"
  - "https://test.pypi.org/simple"
  - "--cache-dir"
  - /tmp/.tmpzx3x5G
env:
  PUFFIN_NO_WRAP: "1"
  VIRTUAL_ENV: /tmp/.tmpG2Gepe/.venv
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    7     7 │ ␊
    8     8 │       Because there are no versions of c<1.0.0 | >1.0.0, <2.0.0 | >2.0.0 and c==1.0.0 depends on a<2.0.0, c<2.0.0 depends on a<2.0.0.␊
    9     9 │       And because c==2.0.0 depends on a>=3.0.0, c depends on a<2.0.0 | >=3.0.0.␊
   10    10 │       And because a<2.0.0 depends on b==1.0.0 (1), a!=3.0.0, c*, b!=1.0.0 are incompatible.␊
   11       │-      And because a==3.0.0 depends on b==3.0.0, c depends on b<=1.0.0 | >=3.0.0.␊
   12       │-      And because root depends on c and root depends on b>=2.0.0, <3.0.0, version solving failed.
         11 │+      And because a==3.0.0 depends on b==3.0.0, c depends on b==1.0.0 | ==3.0.0.␊
         12 │+      And because root depends on c and root depends on b>=2.0.0, <3.0.0, version solving failed.␊
────────────┴───────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'dependency_excludes_range_of_compatible_versions' panicked at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.34.0/src/runtime.rs:563:9:
snapshot assertion for 'dependency_excludes_range_of_compatible_versions' failed in line 243
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

I then pushed no-op commit, and it succeeded: https://github.com/astral-sh/puffin/actions/runs/7468378577/job/20323700268.

---

_Label `bug` added by @charliermarsh on 2024-01-10 00:10_

---

_Comment by @zanieb on 2024-01-10 00:14_

Ah interesting the diff is at `c depends on b<=1.0.0 | >=3.0.0` and `c depends on b==1.0.0 | ==3.0.0`

---

_Assigned to @BurntSushi by @BurntSushi on 2024-01-22 16:46_

---

_Closed by @charliermarsh on 2024-01-28 14:27_

---
