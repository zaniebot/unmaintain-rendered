```yaml
number: 7847
title: "Accept `git+` prefix in `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/git-plus
created_at: 2024-10-01T15:54:51Z
updated_at: 2024-10-14T09:13:49Z
url: https://github.com/astral-sh/uv/pull/7847
synced_at: 2026-01-10T12:53:57Z
```

# Accept `git+` prefix in `tool.uv.sources`

---

_Pull request opened by @charliermarsh on 2024-10-01 15:54_

## Summary

Right now, this fails, because we later try to do `git+git+https://...`. It seems nice to have this "just work".


---

_Label `bug` added by @charliermarsh on 2024-10-01 15:54_

---

_Marked ready for review by @charliermarsh on 2024-10-01 15:54_

---

_Merged by @charliermarsh on 2024-10-01 16:12_

---

_Closed by @charliermarsh on 2024-10-01 16:12_

---

_Branch deleted on 2024-10-01 16:12_

---

_Comment by @salty-horse on 2024-10-06 11:17_

This change meant that uv 0.4.18 can't `sync --frozen` from a lock file created with 0.4.17.

I don't know if backwards compatibility is desired at this point, but mentioning it just in case.

---

_Comment by @charliermarsh on 2024-10-06 15:38_

Do you mind sharing a brief reproduction?

---

_Comment by @charliermarsh on 2024-10-06 15:39_

(We use minor versions for breaking changes so this wasn’t intended to be breaking!)

---

_Comment by @salty-horse on 2024-10-06 16:26_

The important thing is to run `uv cache clean`. Otherwise, it won't attempt to re-fetch the repository from the URL that's in a format it no longer understands.

I used this on a random repository. It will work with anything you have git access to.
```
$ cat pyproject.toml 
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
	"apns2",
]

[tool.uv.sources]
apns2 = {git = "git+ssh://git@github.com/salty-horse/PyAPNs2.git", rev = "httpx_cert_auth"}

$ curl -LsSf https://astral.sh/uv/0.4.17/install.sh | sh
$ uv --version
uv 0.4.17
$ rm -rf .venv
$ uv sync
# Worked successfully. Important bit:
#  + apns2==0.7.5 (from git+git+ssh://github.com/salty-horse/PyAPNs2.git@9ce3e36a8bbed52e5e48d66ce2fe80eee0b9f702)

$ curl -LsSf https://astral.sh/uv/0.4.18/install.sh | sh
$ uv --version
uv 0.4.18
$ rm -rf .venv
$ uv cache clean
$ uv sync --frozen
[...]
Creating virtual environment at: .venv
Updating git+ssh://github.com/salty-horse/PyAPNs2.git (httpx_cert_auth)                                                                                                                                                                       error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: apns2 @ git+git+ssh://github.com/salty-horse/PyAPNs2.git@9ce3e36a8bbed52e5e48d66ce2fe80eee0b9f702
  Caused by: Git operation failed
  Caused by: failed to clone into: /home/ori/.cache/uv/git-v0/db/a4d58569fd76b658
  Caused by: failed to fetch commit `9ce3e36a8bbed52e5e48d66ce2fe80eee0b9f702`
  Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'git+ssh://github.com/salty-horse/PyAPNs2.git' '+9ce3e36a8bbed52e5e48d66ce2fe80eee0b9f702:refs/commit/9ce3e36a8bbed52e5e48d66ce2fe80eee0b9f702'` (exit status: 128)
--- stderr
ori@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

---

_Comment by @charliermarsh on 2024-10-06 16:46_

Thanks! I don’t understand why this worked in the 0.4.17 case — that was the whole point of the commit hah. I must’ve overlooked something. I’ll look into it when I’m back from vacation.

---

_Comment by @salty-horse on 2024-10-06 16:55_

I may be commenting on the wrong PR :flushed: - it seemed relevant.

The patch seems to handle the URLs in `pyproject.toml`, but the issue is with the URLs in `uv.lock`.

The old version recorded the URL with `git+` in `uv.lock`:
```
source = { git = "git+ssh://git@github.com/
```
The newer version records it without `git+`:
```
source = { git = "ssh://git@github.com/
```
So when it reads the file from the old version, it assumes `git+` isn't there, and adds it, which causes problems down the line. It should check before adding the prefix.

Enjoy your vacation!

---

_Comment by @charliermarsh on 2024-10-09 08:44_

Ahh ok, I think I understand. Prior to that change, we allowed `source = { git = "git+ssh://git@github.com/..." }` but errored on `source = { git = "https+ssh://git@github.com/..." }`. So this likely was subtly breaking for `git+ssh` usages.

---

_Comment by @salty-horse on 2024-10-14 09:13_

Should I open a new issue about the regression?

---
