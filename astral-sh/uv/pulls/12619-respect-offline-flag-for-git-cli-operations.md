```yaml
number: 12619
title: respect --offline flag for git cli operations
type: pull_request
state: merged
author: thejchap
labels: []
assignees: []
merged: true
base: main
head: bug/git-cli-offline
created_at: 2025-04-02T04:49:57Z
updated_at: 2025-04-04T22:18:47Z
url: https://github.com/astral-sh/uv/pull/12619
synced_at: 2026-01-10T11:10:40Z
```

# respect --offline flag for git cli operations

---

_Pull request opened by @thejchap on 2025-04-02 04:49_

## Summary
closes #12234

[fetch_with_cli](https://github.com/thejchap/uv/blob/e0f81f0d4a904d8c743e776d9f8c9ef5b96f769c/crates/uv-git/src/git.rs#L573) doesn't respect the registry client's [connectivity setting](https://github.com/thejchap/uv/blob/e0f81f0d4a904d8c743e776d9f8c9ef5b96f769c/crates/uv-client/src/registry_client.rs#L1009) - this pr updates `fetch_with_cli` to set `GIT_ALLOW_PROTOCOL=file` when the client's connectivity setting is `Connectivity::Offline`

## Test Plan
E2E

```sh
cargo run add "pycurl @ git+https://github.com/pycurl/pycurl.git" --directory ~/src/offline-test/ --offline
```

```sh
   Compiling uv-cli v0.0.1 (/Users/justinchapman/src/uv/crates/uv-cli)
   Compiling uv v0.6.11 (/Users/justinchapman/src/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.47s
     Running `target/debug/uv add 'pycurl @ git+https://github.com/pycurl/pycurl.git' --directory /Users/justinchapman/src/offline-test/ --offline`
   Updating https://github.com/pycurl/pycurl.git (HEAD)                                                                                                                                   × Failed to download and build `pycurl @ git+https://github.com/pycurl/pycurl.git`
  ├─▶ Git operation failed
  ├─▶ failed to fetch into: /Users/justinchapman/.cache/uv/git-v0/db/9a596e5213c3162d
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://github.com/pycurl/pycurl.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      fatal: transport 'https' not allowed

  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

---

_Marked ready for review by @thejchap on 2025-04-02 04:51_

---

_@zanieb reviewed on 2025-04-02 13:50_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:235 on 2025-04-02 13:50_

Could we just pass the connectivity into here instead of a bool?

---

_@zanieb reviewed on 2025-04-02 13:51_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:235 on 2025-04-02 13:51_

I guess we pass bools for `disable_ssl`, but I presume that's because the type itself is more complex. I don't feel strongly, but less bools is nice.

---

_@zanieb reviewed on 2025-04-02 13:52_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:495 on 2025-04-02 13:52_

```suggestion
    /// Used to set allowed protocols for git operations.
    ///
    /// When uv is in "offline" mode, only the "file" protocol is allowed.
```

---

_Comment by @zanieb on 2025-04-02 13:52_

Cool!

Would it be feasible to detect the message

> fatal: transport 'https' not allowed

and wrap the error with something more informative?

Could you add a test case in https://github.com/astral-sh/uv/blob/ddbf90c906083acd51afd86409fa3cb1ae2bcaa6/crates/uv/tests/it/pip_install.rs


---

_Assigned to @zanieb by @zanieb on 2025-04-02 16:19_

---

_@thejchap reviewed on 2025-04-03 05:40_

---

_Review comment by @thejchap on `crates/uv-git/src/git.rs`:235 on 2025-04-03 05:40_

@zanieb will defer to you! only passed in as a bool because it meant not having to add an additional import to `uv-git` to pull in the type (`Connectivity` is defined in `uv-client`)

---

_@zanieb reviewed on 2025-04-03 13:07_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:235 on 2025-04-03 13:07_

Ah okay, we can skip that then for now

---

_Comment by @thejchap on 2025-04-04 06:41_

@zanieb test added and tweaked the error message

---

_@zanieb approved on 2025-04-04 14:12_

Thank you!

---

_Merged by @zanieb on 2025-04-04 16:02_

---

_Closed by @zanieb on 2025-04-04 16:02_

---

_Comment by @thejchap on 2025-04-04 22:18_

Thanks for the help @zanieb !

---
