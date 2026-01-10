```yaml
number: 3471
title: netrc authentication seems to fail for index_url specified in requirements.txt.
type: issue
state: closed
author: likesum
labels:
  - external
assignees: []
created_at: 2024-05-08T20:00:15Z
updated_at: 2024-10-04T20:59:20Z
url: https://github.com/astral-sh/uv/issues/3471
synced_at: 2026-01-10T04:45:09Z
```

# netrc authentication seems to fail for index_url specified in requirements.txt.

---

_Issue opened by @likesum on 2024-05-08 20:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

**Description:**
`netrc` authentication seems to fail for `index_url` specified in `requirements.txt`.

**How to reproduce:**
`uv pip install -r requirement.txt` does not work if `requirement.txt` contains an `--index-url` whose authentication is stored in `~/.netrc`. 
```
error: HTTP status client error (401 Unauthorized) for url
```

It works, however, if one moves the url out of the txt file and does `uv pip install --index-url url_without_authentication -r requirement.txt`.

**The current uv platform**: Linux.

**uv version**: 0.1.38.

---

_Comment by @zanieb on 2024-05-08 20:51_

Interesting... Can you share `RUST_LOG=uv=trace uv pip install -v ...` logs for the installation? Ensure you replace your password with asterisks in the logs.

---

_Comment by @likesum on 2024-05-09 00:57_

> Interesting... Can you share `RUST_LOG=uv=trace uv pip install -v ...` logs for the installation? Ensure you replace your password with asterisks in the logs.

Thanks for the fast response! I assume you are asking for the logs for `uv pip install -r requirement.txt`?
```
TRACE No cache entry exists for /home/****/.cache/uv/simple-v7/89b815c496164a1b/absl-py.rkyv
DEBUG No cache entry for: https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Sending fresh GET request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Handling request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Attempting unauthenticated request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
...
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/gitpython/ failed with 401 Unauthorized, checking for credentials
```

FWIW, directly use `pip install` instead of `uv pip install` works.

---

_Comment by @zanieb on 2024-05-09 01:43_

We're missing the relevant logs there, we don't try the netrc file until the request fails so where you stopped at "failed with 401 Unauthorized, checking for credentials" is the important part :)

---

_Comment by @likesum on 2024-05-09 02:06_

Lol ok! It basically failed and exit at that point, so I didn't include it. Here it is:
```
TRACE No cache entry exists for /home/****/.cache/uv/simple-v7/89b815c496164a1b/absl-py.rkyv
DEBUG No cache entry for: https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Sending fresh GET request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Handling request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Attempting unauthenticated request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
...
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/gitpython/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://****
error: HTTP status client error (401 Unauthorized) for url (https://****/artifactory/api/pypi/pypi-remote/simple/gitpython/)
```

---

_Comment by @zanieb on 2024-05-09 02:18_

Hm so we're not seeing this log 

https://github.com/astral-sh/uv/blob/c1370cab1b153a3d45c82cc2afe414cf2e1b2c82/crates/uv-auth/src/middleware.rs#L333

which suggests to me that we're not discovering the netrc file which is pretty weird. Would it be feasible to share the full logs in a gist or something?


---

_Assigned to @zanieb by @zanieb on 2024-05-09 02:21_

---

_Comment by @likesum on 2024-05-09 02:22_

This is basically the full log. I removed all entries in the `requirements.txt` except one. And here is the full log:
```
INFO Found a virtualenv through CONDA_PREFIX at: /home/****/.dotfiles/mambaforge/envs/sandbox
DEBUG Cached interpreter info for Python 3.10.14, skipping probing: /home/****/.dotfiles/mambaforge/envs/sandbox/bin/python
DEBUG Using Python 3.10.14 environment at /home/****/.dotfiles/mambaforge/envs/sandbox/bin/python
DEBUG Trying to lock if free: /tmp/uv-d91bf38ff76b0a64.lock
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.10.14
DEBUG Adding direct dependency: absl-py>=1.2.0
TRACE Fetching metadata for absl-py from https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE No cache entry exists for /home/****/.cache/uv/simple-v7/89b815c496164a1b/absl-py.rkyv
DEBUG No cache entry for: https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Sending fresh GET request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Handling request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Attempting unauthenticated request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/
TRACE Request for https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://****
error: HTTP status client error (401 Unauthorized) for url (https://****/artifactory/api/pypi/pypi-remote/simple/absl-py/)
```

Or do you want me to try something else?

---

_Comment by @zanieb on 2024-05-09 13:20_

This is helpful, thank you. I'll look into it on my end and report back.

---

_Comment by @zanieb on 2024-05-09 14:24_

Interestingly, I added a test case for this and it succeeds: https://github.com/astral-sh/uv/pull/3485

Does it look meaningfully different than your setup?

---

_Comment by @chrisirhc on 2024-08-08 00:06_

In case this might help, in my scenario, the `$HOME` environment variable wasn't populated and hence, uv did not detect the .netrc file, whereas pypi is able to find and use it.

Opened https://github.com/gribouille/netrc/issues/4 in upstream.

---

_Label `upstream` added by @zanieb on 2024-10-04 20:59_

---

_Comment by @zanieb on 2024-10-04 20:59_

Not much to do here until addressed upstream, I think.

---

_Closed by @zanieb on 2024-10-04 20:59_

---
