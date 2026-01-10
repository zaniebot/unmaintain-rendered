---
number: 7453
title: "HTTP Git credentials in `pyproject.toml` are ignored during sync"
type: issue
state: closed
author: flbraun
labels:
  - bug
assignees: []
created_at: 2024-09-17T10:15:30Z
updated_at: 2024-09-18T12:33:02Z
url: https://github.com/astral-sh/uv/issues/7453
synced_at: 2026-01-10T01:24:15Z
---

# HTTP Git credentials in `pyproject.toml` are ignored during sync

---

_Issue opened by @flbraun on 2024-09-17 10:15_

I'm currently evaluating uv for our Python teams. So far I'm very impressed with the overall snappiness (compared to Pipenv), thank you very much for the good work so far. Unfortunately I ran into an issue when dealing with private packages from our Gitlab.  
I don't have knowledge about uv's internal workings, but my guess is that the authentication credentials aren't stored properly at some point and are then missing for successive operations.

```shell
$ uv --version
uv 0.4.10
$ uname -sp
Linux x86_64
```

First, I'm adding the dependency to the project:
```shell
$ RUST_LOG=uv=trace uv add "git+https://user:pass@gitlab.mycompany.com/group/project.git" --tag "v2024.8.6"
[...]
DEBUG Fetching source distribution from Git: https://user:pass@gitlab.mycompany.com/group/project.git
TRACE Checking lock for `https://gitlab.mycompany.com/group/project` at `/tmp/.tmp5Co2Yg/git-v0/locks/924509151ecc8240`
DEBUG Acquired lock for `https://gitlab.mycompany.com/group/project`
DEBUG Updating Git source `https://user:pass@gitlab.mycompany.com/group/project.git`
DEBUG Performing a Git fetch for: https://user:pass@gitlab.mycompany.com/group/project.git
DEBUG reset /tmp/.tmp5Co2Yg/git-v0/checkouts/924509151ecc8240/9bf0934 to 9bf093425188c4feaa7f86a78569b2fbbf5ba1a0
DEBUG Released lock at `/tmp/.tmp5Co2Yg/git-v0/locks/924509151ecc8240`
TRACE Checking lock for `/tmp/.tmp5Co2Yg/built-wheels-v3/git/8742abde34b47f9d/9bf093425188c4fe` at `/tmp/.tmp5Co2Yg/built-wheels-v3/git/8742abde34b47f9d/9bf093425188c4fe/.lock`
DEBUG Acquired lock for `/tmp/.tmp5Co2Yg/built-wheels-v3/git/8742abde34b47f9d/9bf093425188c4fe`
DEBUG No static `pyproject.toml` available for: git+https://user:pass@gitlab.mycompany.com/group/project.git (PyprojectToml(DynamicField("version")))
DEBUG No static `PKG-INFO` available for: git+https://user:pass@gitlab.mycompany.com/group/project.git (MissingPkgInfo)
DEBUG No static `egg-info` available for: git+https://user:pass@gitlab.mycompany.com/group/project.git (MissingEggInfo)
DEBUG Preparing metadata for: git+https://user:pass@gitlab.mycompany.com/group/project.git
[...]
Prepared 97 packages in 6.90s
Installed 97 packages in 179ms
[...]
```

As you can clearly see in the logs the credentials are present and the operation finishes successfully.

The package appears in `pyproject.toml` and `uv.lock`; note that the stated credentials are missing:
```ini
# pyproject.toml
[project]
...
dependencies = [
    ...
    "package",
]

[tool.uv.sources]
bxlog = { git = "https://gitlab.mycompany.com/group/project.git", tag = "v2024.8.6" }
```

```ini
# uv.lock
...
[[package]]
name = "thisproject"
...
dependencies = [
    ...
    { name = "package" },
]

[package.metadata]
requires-dist = [
    ...
    { name = "package", git = "https://gitlab.mycompany.com/group/project.git?tag=v2024.8.6" },
]

...
[[package]]
name = "package"
version = "2024.8.6"
source = { git = "https://gitlab.mycompany.com/group/project.git?tag=v2024.8.6#020cae642886a1fce3528366402047d479621904" }
```

Now, let's simulate another developer checking the project out and installing the dependencies for the first time:
```shell
$ rm -rf .venv
$ RUST_LOG=uv=trace uv sync --no-cache
[...]
DEBUG Identified uncached requirement: package @ git+https://gitlab.mycompany.com/group/package.git@v2024.8.6
[...]
DEBUG Fetching source distribution from Git: https://gitlab.mycompany.com/group/package.git
[...]
TRACE Checking lock for `https://gitlab.mycompany.com/group/package` at `/tmp/.tmpcPhaTx/git-v0/locks/924509151ecc8240`
DEBUG Acquired lock for `https://gitlab.mycompany.com/group/package`
[...]
DEBUG Updating Git source `https://gitlab.mycompany.com/group/package.git`
DEBUG Performing a Git fetch for: https://gitlab.mycompany.com/group/package.git
[...]
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: package @ git+https://gitlab.mycompany.com/group/package.git@020cae642886a1fce3528366402047d479621904
  Caused by: Git operation failed
  Caused by: failed to clone into: /tmp/.tmpcPhaTx/git-v0/db/924509151ecc8240
  Caused by: failed to fetch commit `020cae642886a1fce3528366402047d479621904`
  Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'https://gitlab.mycompany.com/group/package.git' '+020cae642886a1fce3528366402047d479621904:refs/remotes/origin/HEAD'` (exit status: 128)
--- stderr
remote: 
remote: ========================================================================
remote: 
remote: The project you were looking for could not be found or you don't have permission to view it.
remote: 
remote: ========================================================================
remote: 
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Looking back at `pyproject.toml`  and `uv.lock` that's an understandable response - the `username:password` tuple is nowhere to be seen, there simply aren't any credentials that uv could have included in the request.

Interestingly, manually adding the credentials back into the URL  in `uv.lock` makes `uv sync` operate as expected, but after the next uv operation that modifies the lock file (e.g. `uv lock --upgrade`) the credentials are lost again, so that's not even a valid workaround.

---

_Comment by @charliermarsh on 2024-09-17 12:59_

We intentionally do not store any credentials in plaintext files by default. You should be able to re-add them to the `pyproject.toml` if you actually want them to be persisted in plaintext.

---

_Comment by @zanieb on 2024-09-17 13:11_

It's generally dangerous to commit credentials to your project, is there a reason that you need to do that?

You might want to look into the [Git credential store](https://git-scm.com/docs/git-credential-store) which we will use automatically if the credentials have been populated. Some similar notes on this exist in the [Cargo documentation](https://doc.rust-lang.org/cargo/appendix/git-authentication.html#https-authentication).

---

_Label `question` added by @zanieb on 2024-09-17 13:11_

---

_Comment by @flbraun on 2024-09-17 13:33_

@charliermarsh Re-adding the credentials to `pyproject.toml` will keep them there, but they will never be included in `uv.lock` which ultimately results in the error described above.

I'm fully aware of the security implications of plaintext secrets. In this case it is fine however, since the project is only used within the company and the credential is a [Gitlab deploy token](https://docs.gitlab.com/ee/user/project/deploy_tokens/), not a personal account. I won't deny that there are better ways of handling this, but that's the way it is right now.

uv's documentation explicitly states support for HTTP authentication, so I'd expect that to work with basic uv workflows without any big surprises:
```
Authentication can come from the following sources, in order of precedence:
- The URL, e.g., https://<user>:<password>@<hostname>/...
```

---

_Comment by @charliermarsh on 2024-09-17 13:35_

If you're seeing the above trace after re-adding the credentials to `pyproject.toml`, then that's a bug, but your summary didn't suggest that you tried that -- only that you re-added them to the `uv.lock`. Did you? uv should not require that the credentials are present in the `uv.lock` file.

---

_Comment by @flbraun on 2024-09-17 14:03_

Sorry for the confusion, let me clear this up:

In my original post I just manually added it to `uv.lock` but not `pyproject.toml`. That worked until I ran `uv lock --upgrade` which removed the credentials from `uv.lock`, leading to the described error when running `uv sync --no-cache`.

After your response I tried adding them to both `pyproject.toml` and `uv.lock`. That also works, but when running `uv lock --upgrade` the credentials are kept in `pyproject.toml` but are removed from `uv.lock`.
With the credentials *only* being in `pyproject.toml` running `uv sync --no-cache` fails to fetch the package again.

If you say that having it only in `pyproject.toml` is the desired state, then yeah, I guess we have a bug here.

---

_Label `question` removed by @zanieb on 2024-09-17 14:31_

---

_Label `bug` added by @zanieb on 2024-09-17 14:31_

---

_Comment by @zanieb on 2024-09-17 14:33_

We have test coverage for this exact situation — I'm surprised this isn't working for you? https://github.com/astral-sh/uv/blob/fe8880bf3cd132bb1c4795f7423dc1628fd3742c/crates/uv/tests/lock.rs#L5999-L6030 

---

_Comment by @zanieb on 2024-09-17 14:38_

Does it work with `--frozen`?

---

_Comment by @charliermarsh on 2024-09-17 14:39_

I think that's different @zanieb -- that tests an index, but here the credentials are provided on the URL directly.

---

_Comment by @zanieb on 2024-09-17 14:42_

Oh good point! We have test coverage for that too, but the test might be wrong https://github.com/astral-sh/uv/pull/7463

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-17 15:25_

---

_Comment by @charliermarsh on 2024-09-17 15:25_

Let's call it a bug for Git credentials then. I'll take a look.

---

_Renamed from "HTTP authentication credentials are forgotten" to "HTTP Git credentials in `pyproject.toml` are ignored during sync" by @zanieb on 2024-09-17 15:54_

---

_Referenced in [astral-sh/uv#7474](../../astral-sh/uv/pulls/7474.md) on 2024-09-17 17:30_

---

_Closed by @charliermarsh on 2024-09-17 19:09_

---

_Comment by @flbraun on 2024-09-18 12:33_

Thank you guys for addressing this so quickly! I'm running 0.4.11 now and everything works as expected.

---
