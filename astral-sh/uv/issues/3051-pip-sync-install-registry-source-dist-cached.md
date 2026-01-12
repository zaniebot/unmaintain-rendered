```yaml
number: 3051
title: "pip_sync: install_registry_source_dist_cached fails on Gentoo amd64"
type: issue
state: closed
author: mgorny
labels:
  - testing
assignees: []
created_at: 2024-04-16T06:17:40Z
updated_at: 2024-04-16T19:07:50Z
url: https://github.com/astral-sh/uv/issues/3051
synced_at: 2026-01-12T15:58:41Z
```

# pip_sync: install_registry_source_dist_cached fails on Gentoo amd64

---

_@mgorny_

After the fix from #2165, I'm seeing one test failure with uv-0.1.32, on Gentoo Linux amd64:

```
---- install_registry_source_dist_cached stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: install_registry_source_dist_cached-3
Source: crates/uv/tests/pip_sync.rs:1457
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Removed 616 files for future ([SIZE])
          5 │+Removed 614 files for future ([SIZE])
────────────┴───────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'install_registry_source_dist_cached' panicked at /var/tmp/portage/dev-python/uv-0.1.32/work/cargo_home/gentoo/insta-1.38.0/src/
runtime.rs:563:9:
snapshot assertion for 'install_registry_source_dist_cached-3' failed in line 1457
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Full log: 
[dev-python:uv-0.1.32:20240416-052155.log](https://github.com/astral-sh/uv/files/14987646/dev-python.uv-0.1.32.20240416-052155.log)


---

_Label `testing` added by @charliermarsh on 2024-04-16 13:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-16 17:54_

---

_Comment by @charliermarsh on 2024-04-16 18:16_

I can fix this up real quick.

---

_Closed by @charliermarsh on 2024-04-16 19:07_

---
