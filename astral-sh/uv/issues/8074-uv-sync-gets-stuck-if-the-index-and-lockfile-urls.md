---
number: 8074
title: "`uv sync` gets stuck if the index and lockfile URLs are not reachable and there are dynamic fields in pyproject"
type: issue
state: closed
author: mgab
labels:
  - bug
assignees: []
created_at: 2024-10-10T08:27:37Z
updated_at: 2024-10-10T14:01:22Z
url: https://github.com/astral-sh/uv/issues/8074
synced_at: 2026-01-10T01:24:23Z
---

# `uv sync` gets stuck if the index and lockfile URLs are not reachable and there are dynamic fields in pyproject

---

_Issue opened by @mgab on 2024-10-10 08:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Problem description

`uv sync` gets stuck (I waited for more than 5 minutes before killing it) if:

- there are dynamic fields in pyproject (I tried with the version)
- the index URL with highest priority is not reachable (either `extra-index-url` or `index-url`, either in `~/.config/uv/uv.toml` or in `pyproject.toml`)
- the URLs in the lockfile are not reachable

## Steps to reproduce:

- make sure you do not have any index url configured
- execute `uv sync` in a directory with this [pyproject.toml.txt](https://github.com/user-attachments/files/17324856/pyproject.toml.txt) and this [uv.lock.txt](https://github.com/user-attachments/files/17324857/uv.lock.txt) files (removing the `.txt` suffix I had to add so that github allowed me to upload the files)

If you change the url to a responsive one, it will fail with an error because there will be no git repository to get the version from, no any `src/test_app` package, but that'd be the expected behavior. 

## Versions

Platform: macOS 14.6.1
uv version: uv 0.4.19 (Homebrew 2024-10-07)

## Debug traces

```
❯ uv sync --verbose
DEBUG uv 0.4.19 (Homebrew 2024-10-07)
DEBUG Found project root: `/Users/username/Code/test`
DEBUG No workspace root found, using project root
DEBUG The virtual environment's Python version satisfies `Python >=3.12`
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: test-app @ file:///Users/username/Code/test (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `/Users/username/.cache/uv/sdists-v4/editable/b0b82e1806a4930c`
WARN Failed to read metadata for file: No such file or directory (os error 2)
WARN Failed to read metadata for file: No such file or directory (os error 2)
DEBUG Preparing metadata for: test-app @ file:///Users/username/Code/test
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG No cache entry for: http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/
DEBUG No cache entry for: http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatch-vcs/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Transient request failure for http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/, retrying: error sending request for url (http://nexus.internal.stuart.com:8081/repository/pypi-group/simple/hatchling/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known
DEBUG Released lock at `/Users/username/.cache/uv/sdists-v4/editable/b0b82e1806a4930c/.lock`
DEBUG Ignoring existing lockfile due to missing metadata for: `test-app==0.1.dev1+g85e54b6.d20241010`
DEBUG Starting clean resolution
DEBUG No static `pyproject.toml` available for: test-app @ file:///Users/username/Code/test (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `/Users/username.cache/uv/sdists-v4/editable/b0b82e1806a4930c`
WARN Failed to read metadata for file: No such file or directory (os error 2)
WARN Failed to read metadata for file: No such file or directory (os error 2)
DEBUG Preparing metadata for: test-app @ file:///Users/username/Code/test
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
```


---

_Referenced in [astral-sh/uv#6349](../../astral-sh/uv/issues/6349.md) on 2024-10-10 09:02_

---

_Comment by @charliermarsh on 2024-10-10 09:32_

I see a few retries there, but we should probably error after the third retry per our limits.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-10 09:51_

---

_Label `bug` added by @charliermarsh on 2024-10-10 09:51_

---

_Comment by @charliermarsh on 2024-10-10 11:00_

I see the issue here.

---

_Referenced in [astral-sh/uv#8083](../../astral-sh/uv/pulls/8083.md) on 2024-10-10 11:32_

---

_Closed by @charliermarsh on 2024-10-10 14:01_

---
