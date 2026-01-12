```yaml
number: 3950
title: Fetch git trees on demand
type: pull_request
state: closed
author: ibraheemdev
labels: []
assignees: []
draft: true
base: main
head: partial-clone
created_at: 2024-05-31T21:21:12Z
updated_at: 2025-01-15T19:07:34Z
url: https://github.com/astral-sh/uv/pull/3950
synced_at: 2026-01-12T16:05:56Z
```

# Fetch git trees on demand

---

_@ibraheemdev_

## Summary

Pass `--filter=tree:0` when fetching from a git repository. This allows us to load trees and blobs lazily on checkout, which can drastically reduce the amount of data transferred, especially for large/old repositories.

This seems to be supported [since Git 2.20](https://github.com/git/git/commit/77d503757d6328703f9571a4432dd83800bc26bb), released late 2018.

---

_Converted to draft by @ibraheemdev on 2024-05-31 21:26_

---

_Comment by @ibraheemdev on 2024-05-31 21:58_

Relevant: https://github.com/pypa/pip/issues/11043


---

_Comment by @ibraheemdev on 2024-06-04 00:28_

I'm not really sure why this is failing in CI. The error messages suggest that the Git *server* does not support partial clones, which is surprising considering I have tests passing locally. It's possible that the error message is misleading and the version of Git on the runner does not fully support partial clones, which would also be surprising. I suspect that is also a red herring because pip doesn't seem to have any issues.

---

_Comment by @zanieb on 2024-06-04 13:43_

I can try to look at this too.

---

_Comment by @zanieb on 2024-06-05 13:35_

I tried running tests locally and the suite hangs on `compile_git_mismatched_name` (which is eventually cancelled)

```
❯ git -v
git version 2.39.3 (Apple Git-146)
```

---

_@zanieb reviewed on 2024-06-05 14:17_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 14:17_

If I comment this one out, the test suite doesn't hang 🎉 but there are still a bunch of failures e.g.

```
Snapshot: reinstall_git
Source: crates/uv/tests/pip_sync.rs:2010
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    0       │-success: true
    1       │-exit_code: 0
          0 │+success: false
          1 │+exit_code: 2
    2     2 │ ----- stdout -----
    3     3 │ 
    4     4 │ ----- stderr -----
    5       │-Resolved 1 package in [TIME]
    6       │-Downloaded 1 package in [TIME]
    7       │-Installed 1 package in [TIME]
    8       │- + uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389)
          5 │+error: Failed to download and build `uv-public-pypackage @ git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389`
          6 │+  Caused by: Git operation failed
          7 │+  Caused by: process didn't exit successfully: `git reset --hard b270df1a2fb5d012294e9aaf05e7e0bab1e6a389` (exit status: 128)
          8 │+--- stderr
          9 │+error: unable to read sha1 file of .gitignore (e69de29bb2d1d6434b8b29ae775ad8c2e48c5391)
         10 │+error: unable to read sha1 file of README.md (0a50718e2c98ce585c9f06d68e853e06cc1c307f)
         11 │+error: unable to read sha1 file of dist/uv_public_pypackage-0.1.0-py3-none-any.whl (30de426777a0e739ccadd993ea1313b95f6386b4)
         12 │+error: unable to read sha1 file of dist/uv_public_pypackage-0.1.0.tar.gz (4d8eb8d9e7558239356266c836da3bfca3aca8f1)
         13 │+error: unable to read sha1 file of pyproject.toml (f971ef95a81ef15fed29090cdbd46e779f78d8e3)
         14 │+error: unable to read sha1 file of src/uv_public_pypackage/__init__.py (3dc1f76bc69e3f559bee6253b24fc93acee9e1f9)
         15 │+fatal: Could not reset index file to revision 'b270df1a2fb5d012294e9aaf05e7e0bab1e6a389'.
```

I wonder if the CI failures are that the "local" server doesn't support filtering?

---

_@zanieb reviewed on 2024-06-05 14:27_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 14:27_

In https://github.com/astral-sh/uv/pull/4047/commits/7140a27e38b5e5575c83cb02d3341ea14d4c3890, allowing filtering on the "local" server makes the warning "filtering not recognized by server, ignoring" go away.



---

_@zanieb reviewed on 2024-06-05 14:27_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 14:27_

However, if I set that option locally the test suite still hangs on my machine.

---

_@zanieb reviewed on 2024-06-05 14:33_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 14:33_

If I revert https://github.com/astral-sh/uv/pull/3950/commits/29839a0a92c59c2546db134d14d73c2c12ee2c35 the test suite hangs on two tests my machine, but the rest of the tests pass. The other hanging test is `pip_entrypoints`.

---

_Comment by @zanieb on 2024-06-05 14:45_

The Ubuntu CI git version is `2.45.1` fyi

---

_@zanieb reviewed on 2024-06-05 14:56_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 14:56_

Here's some very verbose git logs for the erroring command

```
rror: Failed to download and build `uv-public-pypackage @ git+https://github.com/astral-test/uv-public-pypackage@b270df1a2fb5d012294e9aaf05e7e0bab1e6a389`
  Caused by: Git operation failed
  Caused by: process didn't exit successfully: `git reset --hard b270df1a2fb5d012294e9aaf05e7e0bab1e6a389` (exit status: 128)
--- stderr
    common-main.c:55                  version 2.45.1
    common-main.c:56                  start git reset --hard b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
    compat/linux/procinfo.c:170       cmd_ancestry uv <- pip_sync-faf22c <- cargo-nextest <- bash <- Runner.Worker <- Runner.Listener <- node <- runsvc.sh <- systemd
    repository.c:158                  worktree /tmp/.tmpmBoxHO/git-v0/checkouts/8dab139913c4b566/b270df1
    git.c:465               trace: built-in: git reset --hard b270df1a2fb5d012294e9aaf05e7e0bab1e6a389
    git.c:466                         cmd_name reset (reset)
    builtin/reset.c:439               cmd_mode hard
    run-command.c:724                 child_start[0] git -c fetch.negotiationAlgorithm=noop fetch origin --no-tags --no-write-fetch-head --recurse-submodules=no --filter=blob:none --stdin
    run-command.c:657       trace: run_command: git -c fetch.negotiationAlgorithm=noop fetch origin --no-tags --no-write-fetch-head --recurse-submodules=no --filter=blob:none --stdin
    common-main.c:55                  version 2.45.1
    common-main.c:56                  start /usr/lib/git-core/git -c fetch.negotiationAlgorithm=noop fetch origin --no-tags --no-write-fetch-head --recurse-submodules=no --filter=blob:none --stdin
    compat/linux/procinfo.c:170       cmd_ancestry git <- uv <- pip_sync-faf22c <- cargo-nextest <- bash <- Runner.Worker <- Runner.Listener <- node <- runsvc.sh <- systemd
    repository.c:158                  worktree /tmp/.tmpmBoxHO/git-v0/checkouts/8dab139913c4b566/b270df1
    git.c:465               trace: built-in: git fetch origin --no-tags --no-write-fetch-head --recurse-submodules=no --filter=blob:none --stdin
    git.c:466                         cmd_name fetch (reset/fetch)
    run-command.c:724                 child_start[0] 'git-upload-pack '\''/tmp/.tmpmBoxHO/git-v0/db/8dab139913c4b566'\'''
    run-command.c:657       trace: run_command: unset GIT_CONFIG_PARAMETERS GIT_PREFIX; GIT_PROTOCOL=version=2 'git-upload-pack '\''/tmp/.tmpmBoxHO/git-v0/db/8dab139913c4b566'\'''
    common-main.c:55                  version 2.45.1
    common-main.c:56                  start git-upload-pack /tmp/.tmpmBoxHO/git-v0/db/8dab139913c4b566
    compat/linux/procinfo.c:170       cmd_ancestry sh <- git <- git <- uv <- pip_sync-faf22c <- cargo-nextest <- bash <- Runner.Worker <- Runner.Listener <- node <- runsvc.sh <- systemd
    git.c:465               trace: built-in: git upload-pack /tmp/.tmpmBoxHO/git-v0/db/8dab139913c4b566
    git.c:466                         cmd_name upload-pack (reset/fetch/upload-pack)
    usage.c:64                        error remote error: upload-pack: not our ref 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa
fatal: remote error: upload-pack: not our ref 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa
    usage.c:78                        exit elapsed:0.033049 code:128
    trace2/tr2_tgt_normal.c:128       atexit elapsed:0.033058 code:128
    usage.c:64                        error git upload-pack: not our ref 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa
    run-command.c:977                 child_exit[0] pid:33450 code:128 elapsed:0.039020
fatal: git upload-pack: not our ref 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa
    usage.c:78                        exit elapsed:0.012012 code:128
    trace2/tr2_tgt_normal.c:128       atexit elapsed:0.012018 code:128
    usage.c:64                        error could not fetch 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa from promisor remote
fatal: could not fetch 732879a9c8f6a3df2d7f6c9b7c5a17896b9ec2fa from promisor remote
    usage.c:78                        exit elapsed:0.041203 code:128
    trace2/tr2_tgt_normal.c:128       atexit elapsed:0.041236 code:128
```

https://github.com/astral-sh/uv/actions/runs/9386288785/job/25846511187?pr=4047

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:368 on 2024-06-05 16:37_

Continuing this commentary....

If I upgrade my local git client the tests fail

```
❯ git -v
git version 2.45.2
❯ ct allowed_transitive_git_dependency
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.14s
    Starting 1 test across 52 binaries (1008 skipped; run ID: f9d63952-2ca1-4121-9989-2d78257fbdc3, nextest profile: default)
        FAIL [   1.171s] uv::pip_compile allowed_transitive_git_dependency
error: test run failed

❯ export PATH="/usr/bin:$PATH"
❯ git -v
git version 2.39.3 (Apple Git-146)
❯ ct allowed_transitive_git_dependency
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.14s
    Starting 1 test across 52 binaries (1008 skipped; run ID: f9d63952-2ca1-4121-9989-2d78257fbdc3, nextest profile: default)
        PASS [   6.603s] uv::pip_compile allowed_transitive_git_dependency
------------
     Summary [   6.603s] 1 test run: 1 passed, 1008 skipped
```

so it looks like it's actually a problem with _newer_ git clients.

---

_@zanieb reviewed on 2024-06-05 16:37_

---

_Comment by @zanieb on 2025-01-15 19:07_

Gonna close this as stale / in-actionable. 

---

_Closed by @zanieb on 2025-01-15 19:07_

---
