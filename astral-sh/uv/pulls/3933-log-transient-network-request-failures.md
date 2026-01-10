```yaml
number: 3933
title: Log transient network request failures
type: pull_request
state: merged
author: konstin
labels:
  - error messages
  - tracing
assignees: []
merged: true
base: main
head: konsti/log-transient-request-failures
created_at: 2024-05-31T09:48:00Z
updated_at: 2024-06-04T13:39:17Z
url: https://github.com/astral-sh/uv/pull/3933
synced_at: 2026-01-10T13:54:02Z
```

# Log transient network request failures

---

_Pull request opened by @konstin on 2024-05-31 09:48_

We retry several kinds of network request failures, but it's often unclear whether a request was retried or not (https://github.com/astral-sh/uv/issues/3514#issuecomment-2105485773). This PR adds a small intermediary layer that logs all transient request failures, adding the `DEBUG Transient request failure` lines:

```
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `/home/konsti/projects/uv/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: tqdm
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: tqdm*
DEBUG No cache entry for: https://pypi.org/simple/tqdm/
DEBUG Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
DEBUG Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
DEBUG Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
DEBUG Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
error: Could not connect, are you offline?
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
```

I decided for multi-line logging to show the complete error trace since only `Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)` doesn't tell you the actual problem (a dns error).

Note that running with `-v` will not show messages about retry backoff timing, but running with `RUST_LOG=debug` now shows a complete picture:

```
DEBUG starting new connection: https://pypi.org/
DEBUG resolving host="pypi.org"
DEBUG Transient request failure for https://pypi.org/simple/tqdm/, retrying: Request error: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: error sending request for url (https://pypi.org/simple/tqdm/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name or service not known
  Caused by: failed to lookup address information: Name or service not known
WARN Retry attempt #2. Sleeping 528.728192ms before the next attempt
```

Fixes #3572


---

_Label `error messages` added by @konstin on 2024-05-31 09:48_

---

_@zanieb approved on 2024-06-04 13:26_

---

_Label `tracing` added by @zanieb on 2024-06-04 13:26_

---

_Merged by @konstin on 2024-06-04 13:39_

---

_Closed by @konstin on 2024-06-04 13:39_

---

_Branch deleted on 2024-06-04 13:39_

---
