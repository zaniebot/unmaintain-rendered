```yaml
number: 11744
title: Disable interactive git terminal prompts during fetches
type: pull_request
state: merged
author: piegamesde
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2025-02-24T11:52:38Z
updated_at: 2025-02-26T03:44:17Z
url: https://github.com/astral-sh/uv/pull/11744
synced_at: 2026-01-12T16:09:59Z
```

# Disable interactive git terminal prompts during fetches

---

_@piegamesde_

## Summary

The animation shadows any interactive authentication prompt which may occur when resolving dependencies of private repos.

Fixes https://github.com/astral-sh/uv/issues/5107.

## Test Plan

I started creating `install_git_private_https_interactive` as a regression test but am unsure how to test this because it is interactive and I don't really know the test framework


---

_Comment by @piegamesde on 2025-02-24 12:06_

It looks like the test drive is having at least some sort of git credentials configuration leaking through global user state. If I run it naively, it fails directly:

```
   Updating https://github.com/astral-test/uv-private-pypackage (HEAD)                                                                                                     × Failed to download and build `uv-private-pypackage @ git+https://github.com/astral-test/uv-private-pypackage`
  ├─▶ Git operation failed
  ├─▶ failed to clone into: /home/piegames/.cache/uv/git-v0/db/8401f5508e3e612d
  ╰─▶ process didn't exit successfully: `/home/piegames/.nix-profile/bin/git fetch --force --update-head-ok 'https://github.com/astral-test/uv-private-pypackage'
      '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      remote: Repository not found.
      fatal: repository 'https://github.com/astral-test/uv-private-pypackage/' not found
```

But if I run it with my `.git-credentials` file deleted, I instead get the desired password prompt

```
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running `/home/piegames/.cache/cargo-target/debug/uv pip install 'git+https://github.com/astral-test/uv-private-pypackage'`
   Updating https://github.com/astral-test/uv-private-pypackage (HEAD)
Resolving dependencies...                                                                                                                                                Username for 'https://github.com': piegamesde
   Updating https://github.com/astral-test/uv-private-pypackage (HEAD)                                                                                                   error: Git operation failed
  Caused by: failed to fetch into: /home/piegames/.cache/uv/git-v0/db/8401f5508e3e612d
  Caused by: process didn't exit successfully: `/home/piegames/.nix-profile/bin/git fetch --force --update-head-ok 'https://github.com/astral-test/uv-private-pypackage' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
```

(Which then also fails because I didn't use the correct credentials, but that's besides the point)

---

_Comment by @zanieb on 2025-02-24 17:18_

I'm not sure if this is the change we want. Disabling the spinner entirely seems like a loss. It's unclear to me if we even want to _try_ to support interactive authentication here.

> It looks like the test drive is having at least some sort of git credentials configuration leaking through global user state. If I run it naively, it fails directly:

Do we need to just change the git configuration home to a tmpdir or something?

---

_Comment by @zanieb on 2025-02-24 17:18_

cc @jtfmumm since this was on your list of projects

---

_Comment by @piegamesde on 2025-02-25 12:11_

> Disabling the spinner entirely seems like a loss. It's unclear to me if we even want to try to support interactive authentication here.

If the spinner should stay, then interactive auth needs to go. I removed the spinner because it was the easier fix, but with some instructions I can also try to disable interactive auth instead. Keeping this PR as a hotfix in the meantime would also be okay for me.

This issue is a major road block for a project migration to uv, because the project has private dependencies and if these are not set up correctly beforehand the application will seemingly just hang forever.

---

_Comment by @piegamesde on 2025-02-25 13:28_

Turns out that disabling the prompt is pretty easy, and it even works with other interactive auth mechanisms outside of the CLI. This is clearly a better solution.

I'm not sure on whether the env variable should be set to all `git` calls to be sure, or only to the ones with operations which might prompt in the first place. Also, not sure how to make a proper regression test for this

---

_@zanieb reviewed on 2025-02-25 14:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:392 on 2025-02-25 14:27_

Can you restore the change here?

---

_@zanieb reviewed on 2025-02-25 14:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:2183 on 2025-02-25 14:28_

Do we need the token here if we're not going to use it?

---

_@zanieb reviewed on 2025-02-25 14:28_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:2202 on 2025-02-25 14:28_

I'm guessing this succeeds because the credentials are in the git cache. Is there a way to disable that?

---

_Comment by @zanieb on 2025-02-25 14:36_

Thanks for looking into that!

---

_@zanieb reviewed on 2025-02-25 14:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:2202 on 2025-02-25 14:37_

Oh it does fail in CI

```
8       │- + uv-private-pypackage==0.1.0 (from git+https://***@github.com/astral-test/uv-private-pypackage@d780faf0ac91257d4d5a4f0c5a0e4509608c0071)
          5 │+  × Failed to download and build `uv-private-pypackage @ git+[https://github.com/astral-test/uv-private-pypackage`](https://github.com/astral-test/uv-private-pypackage%60)
          6 │+  ├─▶ Git operation failed
          7 │+  ├─▶ failed to clone into: [CACHE_DIR]/git-v0/db/8401f5508e3e612d
          8 │+  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://github.com/astral-test/uv-private-pypackage' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
          9 │+      --- stderr
         10 │+      fatal: could not read Username for 'https://github.com/': terminal prompts disabled
```

Maybe you just need to update the snapshot?

We should also probably special-case this error, but we can track that in a new issue.

---

_Assigned to @zanieb by @zanieb on 2025-02-25 14:37_

---

_@piegamesde reviewed on 2025-02-25 14:51_

---

_Review comment by @piegamesde on `crates/uv/tests/it/pip_install.rs`:2183 on 2025-02-25 14:51_

Well the original idea for the test was to test that login via CLI works, but now that this tests a failure instead I can indeed remove it.

---

_Marked ready for review by @piegamesde on 2025-02-25 16:01_

---

_Renamed from "Remove animated spinner when resolving dependencies" to "Disable interactive git terminal prompts during fetches" by @zanieb on 2025-02-25 19:30_

---

_Label `bug` added by @zanieb on 2025-02-25 19:30_

---

_@zanieb approved on 2025-02-25 19:30_

---

_Merged by @zanieb on 2025-02-25 19:30_

---

_Closed by @zanieb on 2025-02-25 19:30_

---

_@charliermarsh reviewed on 2025-02-25 19:46_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:583 on 2025-02-25 19:46_

@zanieb -- Should this be an `EnvVars`?

---

_@zanieb reviewed on 2025-02-25 19:47_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:583 on 2025-02-25 19:47_

Technically, yeah. Though we don't read it, so it matters less. I can add it.

---

_@samypr100 reviewed on 2025-02-26 03:35_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:583 on 2025-02-26 03:35_

Still good source for documentation purposes internally like we annotate with `#[attr_hidden]` the other git env vars https://github.com/astral-sh/uv/blob/main/crates/uv-static/src/env_vars.rs#L454

---

_@zanieb reviewed on 2025-02-26 03:44_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:583 on 2025-02-26 03:44_

Oh thanks, I didn't notice we were hiding attrs.

---
