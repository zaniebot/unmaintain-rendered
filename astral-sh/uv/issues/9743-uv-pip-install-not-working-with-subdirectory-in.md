---
number: 9743
title: "`uv pip install` not working with `#subdirectory` in monorepo"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2024-12-09T18:26:51Z
updated_at: 2024-12-10T03:42:50Z
url: https://github.com/astral-sh/uv/issues/9743
synced_at: 2026-01-10T01:24:45Z
---

# `uv pip install` not working with `#subdirectory` in monorepo

---

_Issue opened by @jamesbraza on 2024-12-09 18:26_

I have something like this, both in the same GitHub repo:

```toml
# pyproject.toml

[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64"]

[project]
name = "repo-pkga"
dependencies = ["repo-pkgb"]

[tool.uv.sources]
repo-pkgb = {editable = true, path = "pkgb"}
repo-pkga = {workspace = true}
```

```toml
# pkgb/pyproject.toml

[build-system]
build-backend = "setuptools.build_meta"
requires = ["setuptools>=64"]

[project]
name = "repo-pkgb"
dependencies = []
```

With `uv==0.5.7`, `uv pip install` fails but `pip install` succeeds when passed:
- `"repo-pkga @ git+ssh://git@github.com/org/repo.git"`
- `"repo-pkgb @ git+ssh://git@github.com/org/repo.git#subdirectory=pkgb"`

I believe what is happening is `uv pip` is not seeing `#subdirectory` in the second install and thinks there is a conflict.

<details>
<summary><code>uv pip install</code> Fails</summary>

```none
> uv pip install "repo-pkga @ git+ssh://git@github.com/org/repo.git" "repo-pkgb @ git+ssh://git@github.com/org/repo.git#subdirectory=pkgb"
 Updated ssh://git@github.com/org/repo.git (ec479a8)
  × Failed to download and build `repo-pkga @ git+ssh://git@github.com/org/repo.git`
  ╰─▶ Package metadata name `repo-pkgb` does not match given name `repo-pkga`
```

</details>

<details>
<summary><code>pip install</code> Succeeds</summary>

```none
> pip install "repo-pkga @ git+ssh://git@github.com/org/repo.git" "repo-pkgb @ git+ssh://git@github.com/org/repo.git#subdirectory=pkgb"
Collecting repo-pkga@ git+ssh://****@github.com/org/repo.git
  Cloning ssh://****@github.com/org/repo.git to /private/var/folders/5c/20jqnfqx4sv1_6_bdkf765cr0000gn/T/pip-install-816hulg5/repo-pkga_793bf123f2524b72bb07f205e40b1397
  Running command git clone --filter=blob:none --quiet 'ssh://****@github.com/org/repo.git' /private/var/folders/5c/20jqnfqx4sv1_6_bdkf765cr0000gn/T/pip-install-816hulg5/repo-pkga_793bf123f2524b72bb07f205e40b1397
  Resolved ssh://****@github.com/org/repo.git to commit ec479a820fc45779b40fb94b07f39ebef412ba58
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting repo-pkgb@ git+ssh://****@github.com/org/repo.git#subdirectory=paperscraper
  Cloning ssh://****@github.com/org/repo.git to /private/var/folders/5c/20jqnfqx4sv1_6_bdkf765cr0000gn/T/pip-install-816hulg5/repo-pkgb_22e0edec781045e0a79b343882ee612d
  Running command git clone --filter=blob:none --quiet 'ssh://****@github.com/org/repo.git' /private/var/folders/5c/20jqnfqx4sv1_6_bdkf765cr0000gn/T/pip-install-816hulg5/repo-pkgb_22e0edec781045e0a79b343882ee612d
  Resolved ssh://****@github.com/org/repo.git to commit ec479a820fc45779b40fb94b07f39ebef412ba58
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
```

</details>

---

_Label `bug` added by @zanieb on 2024-12-09 18:46_

---

_Comment by @charliermarsh on 2024-12-09 18:55_

We'll take a look, thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-09 19:13_

---

_Comment by @charliermarsh on 2024-12-09 21:53_

Unfortunately I can't replicate this... Here's my attempt, you can look at the linked repo [here](https://github.com/astral-sh/workspace-with-root-dependency-test):

```
❯ cargo run pip install "uv-git-workspace-in-root @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git" "workspace-member-in-subdir @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git#subdirectory=workspace-member-in-subdir"

    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip install 'uv-git-workspace-in-root @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git' 'workspace-member-in-subdir @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git#subdirectory=workspace-member-in-subdir'`
 Updated ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git (3764e24)
Resolved 2 packages in 903ms
   Built uv-git-workspace-in-root @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git@3764e24efa81ad3151bf068b8ac809569ae2a08e
   Built workspace-member-in-subdir @ git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git@3764e24efa81ad3151bf068b8ac809569ae2a08e#subdirectory=workspace-member-in-subdir
Prepared 2 packages in 564ms
Installed 2 packages in 1ms
 + uv-git-workspace-in-root==0.1.0 (from git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git@3764e24efa81ad3151bf068b8ac809569ae2a08e)
 + workspace-member-in-subdir==0.1.0 (from git+ssh://git@github.com/astral-sh/workspace-with-root-dependency-test.git@3764e24efa81ad3151bf068b8ac809569ae2a08e#subdirectory=workspace-member-in-subdir)
```

---

_Comment by @charliermarsh on 2024-12-09 21:54_

I tried to emulate it as closely as I could. (It doesn't use a workspace despite the name.) If you're able to create an open source reproduction, I'm happy to take another look.

---

_Label `bug` removed by @charliermarsh on 2024-12-09 21:54_

---

_Label `needs-mre` added by @charliermarsh on 2024-12-09 21:54_

---

_Referenced in [astral-sh/workspace-with-root-dependency-test#1](../../astral-sh/workspace-with-root-dependency-test/pulls/1.md) on 2024-12-09 23:48_

---

_Comment by @jamesbraza on 2024-12-09 23:51_

Thank you for getting the repro started @charliermarsh , I appreciate it. I completed it here: https://github.com/astral-sh/workspace-with-root-dependency-test/pull/1

Sorry it's not totally minimal, I was also trying to mirror my exact set up to give you more context. It seems to be related to a combination of SCM versioning and the package in the `#subdirectory` having a dependency.

---

_Comment by @charliermarsh on 2024-12-10 00:06_

Awesome, thank you. I'll revisit it!

---

_Comment by @charliermarsh on 2024-12-10 00:42_

Definitely a bug here.

---

_Label `needs-mre` removed by @charliermarsh on 2024-12-10 00:42_

---

_Label `bug` added by @charliermarsh on 2024-12-10 00:42_

---

_Comment by @charliermarsh on 2024-12-10 00:53_

The proximate cause is that building `workspacemember` creates `workspace_member.egg-info` in the root -- I think because you have `where = [".."]` or something? So we then read that. But I need to make the code robust to it.

---

_Comment by @jamesbraza on 2024-12-10 01:01_

Wow thanks for looking into this pretty responsively and hunting it down, yeah I had a feeling it was a niche edge case.

---

_Referenced in [astral-sh/uv#9760](../../astral-sh/uv/pulls/9760.md) on 2024-12-10 01:15_

---

_Closed by @charliermarsh on 2024-12-10 02:24_

---

_Closed by @charliermarsh on 2024-12-10 02:24_

---

_Comment by @jamesbraza on 2024-12-10 03:42_

Thank you @charliermarsh , much appreciated and thanks for working through the details with me on this 👍 really great work

---
