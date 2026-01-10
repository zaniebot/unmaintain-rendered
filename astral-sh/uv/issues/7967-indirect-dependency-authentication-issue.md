---
number: 7967
title: Indirect Dependency Authentication Issue
type: issue
state: open
author: alfman99
labels:
  - bug
assignees: []
created_at: 2024-10-07T11:22:10Z
updated_at: 2025-02-24T03:59:10Z
url: https://github.com/astral-sh/uv/issues/7967
synced_at: 2026-01-10T01:24:21Z
---

# Indirect Dependency Authentication Issue

---

_Issue opened by @alfman99 on 2024-10-07 11:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Bug Report: Indirect Dependency Authentication Issue

### Minimal Code Snippet
I am using a private package (`a`) that depends on another private package (`b`). The `b` package is not being fetched correctly when I run `uv sync` because it appears that authentication credentials are not being passed for indirect dependencies and the token isn't being fetched from the `pyproject.toml` instead its being checked on the lock file where there is no token.

### Command Invoked
The command I invoked was:
```bash
uv sync --frozen --no-install-project --no-dev
```

### Current uv Platform
- **Platform**: Docker
- **Docker Image**: `ubuntu:noble`
- `uv` installed through `COPY --from=ghcr.io/astral-sh/uv:0.4.18 /uv /bin/uv`

### Current uv Version
Version: **0.4.18**

### Error Message
The output of the command includes the following error:
```
error: Failed to prepare distributions
Caused by: Failed to fetch wheel: b @ git+https://gitlab.com/<redacted>/backend/b.git@<commit_hash>
Caused by: Git operation failed
Caused by: failed to clone into: /root/.cache/uv/git-v0/db/<some_id>
Caused by: failed to fetch commit <commit_hash>
Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'https://gitlab.com/<redacted>/backend/b.git' '+<commit_hash>:refs/remotes/origin/HEAD'` (exit status: 128)
fatal: could not read Username for 'https://gitlab.com': No such device or address
```

### Additional Context
- The `a` package is correctly set up to include the credentials in its `pyproject.toml`, but these credentials do not seem to propagate to the `b` dependency during the `uv sync` process.
- I have verified that the credentials are correct and are functioning when manually installed.

---

_Comment by @charliermarsh on 2024-10-08 22:42_

Are you able to put together a more complete reproduction? Like something I can Git clone or copy-paste to get into this same error state? Would help a lot.

---

_Label `needs-mre` added by @charliermarsh on 2024-10-08 22:43_

---

_Comment by @alfman99 on 2024-10-09 09:37_

Sure, here you go:

There are 3 repos:
- [repo-main](https://github.com/alfman99/repo-main.git)
- [private-repo-a](https://github.com/alfman99/private-repo-a.git)
- [private-repo-b](https://github.com/alfman99/private-repo-b.git)

`repo-main` is public so you will have to clone it. This would be our "application". This application has one direct dependency, `private-repo-a`, and one indirect dependency, `private-repo-b`.

`repo-main` has in its `pyproject.toml` an access token to access the `private-repo-a`, and `private-repo-a` has an access token to access `private-repo-b`. Whenever you try to `uv sync` to run the "application", you will find that `private-repo-b` isn't possible to clone because of the error: `fatal: could not read Username for 'https://github.com': No such device or address`

It works alright if you are logged in with an account that has access to both repos, but whenever you try to do this in an environment like Docker, where it relies purely on the access token, it does not work.

Let me know if you need anything further.

**Note:** The access token was specifically generated for this demo and only has access to both private repositories with read-only permissions.

---

_Comment by @charliermarsh on 2024-10-09 09:57_

Thank you!

---

_Label `needs-mre` removed by @charliermarsh on 2024-10-09 09:57_

---

_Label `bug` added by @charliermarsh on 2024-10-09 09:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-09 12:31_

---

_Comment by @alfman99 on 2024-11-06 09:43_

Is there an ETA for this? Thank you!

---

_Comment by @bhughes339 on 2024-11-20 14:45_

I'm having the same issue, and I'd like to add that the [environment variable method](https://docs.astral.sh/uv/configuration/indexes/#providing-credentials) (using `UV_INDEX_<INDEX_NAME>_USERNAME` and `UV_INDEX_<INDEX_NAME>_PASSWORD`) does not seem to work either. Presumably because it is not reading `pyproject.toml` to extract the index name.

---

_Comment by @zyaoj on 2025-01-31 15:30_

Same issue here. Looking for an update. Thanks!

---

_Comment by @charliermarsh on 2025-01-31 15:32_

To clarify, `repo-a` has an access token hard-coded in the `pyproject.toml`?

---

_Comment by @zyaoj on 2025-01-31 16:21_

In our case, we were running `uv sync` in the github ci action within our internal organization, with a dependency to `repo-a` which is another internal repo. No access token is hard-coded anywhere.

---

_Comment by @charliermarsh on 2025-01-31 17:41_

Sorry, how is the access token provided?

---

_Comment by @zyaoj on 2025-02-03 11:05_

Thank you for the quick reply!

Since the failure comes from github actions (ci), the way we can provide access token is using `secrets.GITHUB_TOKEN` ([ref](https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication)). What would be the best way for us to pass this to uv so that we can resolve the internal github repo?

---

_Comment by @charliermarsh on 2025-02-03 17:55_

So in some sense, this isn't uv-specific, right? You would need an access token that has access to a repo other than the repo that you're currently running in? Or am I misunderstanding?

---

_Comment by @alfman99 on 2025-02-04 12:01_

Hey everyone, just to clarify: the issue we are facing happens on local Docker setups, not in GitHub Actions. When running uv sync in Docker, indirect dependencies fail to authenticate because the credentials from pyproject.toml aren’t making it through. 

---

_Comment by @alfman99 on 2025-02-06 11:50_

> To clarify, `repo-a` has an access token hard-coded in the `pyproject.toml`?

Sorry for late reply, just saw it. Yes, `repo-a` has a token hard-coded in the `pyproject.toml`, just like in the example (you can download it through here: `https://demo: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@github.com/alfman99/private-repo-a.git`. If there is anything else let me know.

---

_Comment by @dd-ssc on 2025-02-19 07:44_

We are in a really similar situation: There is a tree of 10+ private (`gitLab.com`) package registries involved, so it's not `a` and `b`, but `a` through `h` (or whatever); the basic issue is the same, though.

This has been one major source of pain in our company since at least 2018 - no matter if old-school `pip` or `poetry` or whatever other tool - it seems none of them get even close to supporting this properly: It would be funny how much extra scripting we've had to do to get this to work, but we stopped laughing a couple of years ago 😖

With `uv`, we see a tool for the first time with the potential to fix this.

We are therefore happy to help with this issue wherever we can.

Thank you @alfman99 for going through the effort of setting up sample repos 👍

On a side note: Maybe we should call them _package registries_ (or just _registries_) to prevent confusion ? _repository_ usually refers to _source code_, not _packages_, doesn't it ?

A few points regarding the issue:
* Hardcoding credentials in `pyproject.toml` is not an option for us (and probably shouldn't for anybody) - that file is under version control, can't leak secrets that way.
* We've started using `uv` only a couple of days ago, so not a lot of experience yet. In terms of authentication, the only truly viable option for us seems to be `keyring` with a 1Password backend - which obviously doesn't concern `uv`, but is third party. We are in the process of evaluating things here as I write this; mixed result thus far.
* We are currently migrating all our projects to `uv`; we've used `poetry` for a couple of years before that. Package registry authentication there is done using a `~/Library/Application Support/pypoetry/auth.toml` file with contents like e.g.
    ```
    [http-basic]
    [http-basic.<self-chosen name for package registry>]
    username = "<not shown here>"
    password = "<not shown here>"
    ```
  While we are happy to move away from `poetry` for countless reasons, that part was somewhat ok and the `gitlab.com` side of things is known to work here - which might help in getting registry auth with `uv` / `keyring` / whatever to work.
* We have a rather strict corporate policy to rotate any secrets at least once a month - and that policy (along with the secrets) is externally audited periodically. Whatever solution we end up implementing, it should not require changing any source code, but configuration only. Just mentioning this because it's been an issue in the past...


---

_Comment by @arkodoescode on 2025-02-24 03:58_

I am having a similar auth issue but not with indirect dependencies, rather a singe direct private dependency. Seems to me the bug is related to the `--frozen` CLI option. If this option is set the client reports `401 Unauthorized`. However not using the frozen option seems to resolve this, I am manually locking all package versions in `pyproject.toml`. 

---
