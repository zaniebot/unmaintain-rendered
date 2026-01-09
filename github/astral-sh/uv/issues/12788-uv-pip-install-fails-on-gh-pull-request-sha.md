---
number: 12788
title: "`uv pip install` fails on GH pull-request SHA"
type: issue
state: closed
author: bewing
labels:
  - needs-mre
assignees: []
created_at: 2025-04-09T21:05:30Z
updated_at: 2025-04-15T14:19:55Z
url: https://github.com/astral-sh/uv/issues/12788
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip install` fails on GH pull-request SHA

---

_Issue opened by @bewing on 2025-04-09 21:05_

### Summary

If you attempt to reference a commit by SHA, against the upstream (not fork) repository, uv fails to resolve. regular pip succeeds.

```shell
$ uv pip install -r requirements.txt -r requirements-dev.txt -e .
    Updated https://github.com/Juniper/py-junos-eznc.git (39bd1cfb007b6adea654baeaf78a26d1ad35f385)
   Updating https://github.com/TheKevJames/coveralls-python.git (0ff8f20fd57f68486a4127b0b7f88a5922b910c3)                                                 error: Git operation failed
  Caused by: failed to find branch, tag, or commit `0ff8f20fd57f68486a4127b0b7f88a5922b910c3`
  Caused by: process didn't exit successfully: `/usr/bin/git rev-parse '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0'` (exit status: 128)
--- stdout
0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0

--- stderr
fatal: ambiguous argument '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'
```

pip success:
```shell
$ pip install -r requirements-dev.txt
<snip>

Collecting git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3 (from -r requirements-dev.txt (line 3))
  Cloning https://github.com/TheKevJames/coveralls-python.git (to revision 0ff8f20fd57f68486a4127b0b7f88a5922b910c3) to /tmp/pip-req-build-x43kzpq8
  Running command git clone --filter=blob:none --quiet https://github.com/TheKevJames/coveralls-python.git /tmp/pip-req-build-x43kzpq8
  Running command git rev-parse -q --verify 'sha^0ff8f20fd57f68486a4127b0b7f88a5922b910c3'
  Running command git fetch -q https://github.com/TheKevJames/coveralls-python.git 0ff8f20fd57f68486a4127b0b7f88a5922b910c3
  Running command git checkout -q 0ff8f20fd57f68486a4127b0b7f88a5922b910c3
  Resolved https://github.com/TheKevJames/coveralls-python.git to commit 0ff8f20fd57f68486a4127b0b7f88a5922b910c3
  Installing build dependencies: started
  Installing build dependencies: finished with status 'done'
  Getting requirements to build wheel: started
  Getting requirements to build wheel: finished with status 'done'
  Preparing metadata (pyproject.toml): started
  Preparing metadata (pyproject.toml): finished with status 'done'
```

The commit does exist, but since it is part of the fork, it doesn't appear to be directly resolvable:  https://github.com/TheKevJames/coveralls-python/commit/0ff8f20fd57f68486a4127b0b7f88a5922b910c3

pip appears to fetch the commit explicitly and then checkout.

### Platform

Windows 11 x86_64 WSL2 Ubuntu 22.04 LTS

### Version

uv 0.6.10

### Python version

Python 3.12.9

---

_Label `bug` added by @bewing on 2025-04-09 21:05_

---

_Renamed from "uv pip install fails on GH pull-request SHA" to "`uv pip install` fails on GH pull-request SHA" by @bewing on 2025-04-09 21:13_

---

_Comment by @charliermarsh on 2025-04-10 14:07_

Sorry, I'm having a little trouble following? You want to be able to do `uv pip install git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3`? That runs without error for me:

```
❯ uv pip install git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
    Updated https://github.com/TheKevJames/coveralls-python.git (0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
Resolved 8 packages in 1.91s
      Built docopt==0.6.2
      Built coveralls @ git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
Prepared 3 packages in 323ms
Installed 3 packages in 4ms
 + coverage==7.8.0
 + coveralls==4.0.1 (from git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
 + docopt==0.6.2
```

---

_Label `bug` removed by @charliermarsh on 2025-04-10 14:08_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-10 14:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-10 14:08_

---

_Comment by @bewing on 2025-04-10 19:39_

I just upgraded to 0.6.14, and still can't.  It appears to be related to the fact that my org is ratelimited with the GH API:

```shell
$ uv version
uv 0.6.14

$ uv cache clean
Clearing cache at: /home/bewing/.cache/uv
Removed 53131 files (700.4MiB)

$ uv pip install git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3 -vv
DEBUG uv 0.6.14
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/bewing/projects/napalm/.venv/bin/python3
DEBUG Found `cpython-3.12.10-linux-x86_64-gnu` at `/home/bewing/projects/napalm/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.10 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
DEBUG Querying GitHub for commit at: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Handling request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 with authentication policy auto
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Attempting unauthenticated request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for realm https://api.github.com
DEBUG No netrc file found
DEBUG GitHub API request failed for: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 (403 Forbidden)
DEBUG Fetching source distribution from Git: https://github.com/TheKevJames/coveralls-python.git
TRACE Checking lock for `https://github.com/thekevjames/coveralls-python` at `/home/bewing/.cache/uv/git-v0/locks/0ab647e443ddb041`
DEBUG Acquired lock for `https://github.com/thekevjames/coveralls-python`
DEBUG Updating Git source `https://github.com/TheKevJames/coveralls-python.git`
   Updating https://github.com/TheKevJames/coveralls-python.git (0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Handling request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 with authentication policy auto
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Attempting unauthenticated request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for realm https://api.github.com
TRACE Skipping fetch of credentials for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3, previous attempt failed
DEBUG Failed to check GitHub HTTP status client error (403 Forbidden) for url (https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
DEBUG Performing a Git fetch for: https://github.com/TheKevJames/coveralls-python.git
DEBUG Released lock at `/home/bewing/.cache/uv/git-v0/locks/0ab647e443ddb041`
DEBUG Released lock at `/home/bewing/projects/napalm/.venv/.lock`
error: Git operation failed
  Caused by: failed to find branch, tag, or commit `0ff8f20fd57f68486a4127b0b7f88a5922b910c3`
  Caused by: process didn't exit successfully: `/usr/bin/git rev-parse '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0'` (exit status: 128)
--- stdout
0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0

--- stderr
fatal: ambiguous argument '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'

```

`--token` is not a valid option for `uv pip`:

```nowrap
$ uv --token ${GH_TOKEN} pip install install git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3 -vv
error: unexpected argument '--token' found

Usage: uv [OPTIONS] <COMMAND>

For more information, try '--help'.

$ uv pip install git+https://github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3 -vv --token ${GH_TOKEN}
error: unexpected argument '--token' found

  tip: to pass '--token' as a value, use '-- --token'

Usage: uv pip install --verbose... <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>|--group <GROUP>>

For more information, try '--help'.

```

`git+https://git:${GH_TOKEN}....` also doesn't work -- the API call is still unauthenticated.



---

_Comment by @jtfmumm on 2025-04-14 07:42_

Would you mind sharing the `-vv` logs when you use `git+https://git:${GH_TOKEN}....`?

---

_Comment by @bewing on 2025-04-14 13:44_

The token credentials are not used to access the Github API, only used during clone operation.

```
$ uv pip install git+https://git:${GH_TOKEN}@github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3 -vv
DEBUG uv 0.6.14
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.12.9, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/home/bewing/Projects/napalm/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: git+https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git@0ff8f20fd57f68486a4127b0b7f88a5922b910c3
DEBUG Querying GitHub for commit at: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Handling request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 with authentication policy auto
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Attempting unauthenticated request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for realm https://api.github.com
DEBUG Checking netrc for credentials for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
DEBUG GitHub API request failed for: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 (403 Forbidden)
DEBUG Fetching source distribution from Git: https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git
TRACE Checking lock for `https://github.com/thekevjames/coveralls-python` at `/home/bewing/.cache/uv/git-v0/locks/0ab647e443ddb041`
DEBUG Acquired lock for `https://github.com/thekevjames/coveralls-python`
DEBUG Updating Git source `https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git`
   Updating https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git (0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Handling request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 with authentication policy auto
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Attempting unauthenticated request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for realm https://api.github.com
TRACE Skipping fetch of credentials for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3, previous attempt failed
DEBUG Failed to check GitHub HTTP status client error (403 Forbidden) for url (https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
DEBUG Performing a Git fetch for: https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Handling request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 with authentication policy auto
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Attempting unauthenticated request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3
TRACE Request for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3 failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for realm https://api.github.com
TRACE Skipping fetch of credentials for https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3, previous attempt failed
DEBUG Failed to check GitHub HTTP status client error (403 Forbidden) for url (https://api.github.com/repos/TheKevJames/coveralls-python/commits/0ff8f20fd57f68486a4127b0b7f88a5922b910c3)
DEBUG Performing a Git fetch for: https://git:<REDACTED>@github.com/TheKevJames/coveralls-python.git
DEBUG Released lock at `/home/bewing/.cache/uv/git-v0/locks/0ab647e443ddb041`
DEBUG Released lock at `/home/bewing/Projects/napalm/.venv/.lock`
error: Git operation failed
  Caused by: failed to find branch, tag, or commit `0ff8f20fd57f68486a4127b0b7f88a5922b910c3`
  Caused by: process didn't exit successfully: `/usr/bin/git rev-parse '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0'` (exit status: 128)
--- stdout
0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0

--- stderr
fatal: ambiguous argument '0ff8f20fd57f68486a4127b0b7f88a5922b910c3^0': unknown revision or path not in the working tree.
Use '--' to separate paths from revisions, like this:
'git <command> [<revision>...] -- [<file>...]'

```

---

_Comment by @charliermarsh on 2025-04-14 13:46_

That sounds correct. They should only be used during the clone operation.

---

_Comment by @bewing on 2025-04-14 13:48_

Is there a global option to use authentication when accessing the GH API?  `--token` is only present in `uv self update` AFAIK, and setting `UV_GITHUB_TOKEN` environment variable also has no effect.

---

_Comment by @bewing on 2025-04-15 14:19_

I will note that adding my github token to .netrc for machine api.github.com did allow the command to complete successfully.

---

_Closed by @bewing on 2025-04-15 14:19_

---
