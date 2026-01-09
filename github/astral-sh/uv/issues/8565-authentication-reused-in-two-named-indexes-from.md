---
number: 8565
title: Authentication reused in two named indexes from the same GitLab
type: issue
state: closed
author: hbeukers
labels:
  - bug
assignees: []
created_at: 2024-10-25T13:33:19Z
updated_at: 2025-01-30T16:22:23Z
url: https://github.com/astral-sh/uv/issues/8565
synced_at: 2026-01-07T13:12:18-06:00
---

# Authentication reused in two named indexes from the same GitLab

---

_Issue opened by @hbeukers on 2024-10-25 13:33_

First, thank you for making uv, we are very happy with it.

Currently, we have a problem with the use of named indexes. 
The behavior showed up in the docker `ghcr.io/astral-sh/uv:0.4.26-python3.11-bookworm-slim` on GitLab for running the pipelines.

Our company has a GitLab with two different indexes from which we download packages.

Our project looks something like this:
```toml
[project]
name = "mypackage"
dependencies = [
    "firstpackage>=1.0.0",
    "secondpackage>=1.0.0",
]
requires-python = ">=3.11"

[tool.uv.sources]
firstpackage = { index = "firstindex" }
secondpackage = { index = "secondindex" }

[[tool.uv.index]]
name = "firstindex"
url = "https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple"
explicit = true

[[tool.uv.index]]
name = "secondindex"
url = "https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple"
explicit = true
```

Setting `UV_INDEX_FIRSTPACKAGE_PASSWORD` and `UV_INDEX_SECONDPACKAGE_PASSWORD` (and the usernames) gave the following error:

`Creating virtual environment at: .venv
error: Failed to download `secondpackage==1.0.0`
  Caused by: HTTP status client error (404 Not Found) for url (https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804)`

By playing around with the access tokens I found the following structure. We can make tokens specific for an index or general and valid for both.
The following token combinations give the following results
- first: specific, second: specific -> error
- first: general, second: specific -> works
- first: specific, second: general -> error
- first: general, second: general -> works
- first: general, second: no token set -> works

It also works if I remove firstpackage and just use a specific token for the secondpackage.

It seems that the authentication of the first index is also used for the second one. We can now solve it by using a token that is valid for both, but we would prefer to use tokens that are specific for each index. 

Is this something that is in line with how it was intended? 
Are there additional tests/outputs you would need to understand the behavior?


---

_Comment by @charliermarsh on 2024-10-25 13:36_

I think @zanieb is most qualified to answer this since I'm guessing it has to do with the fact that the indexes use the same realm.

---

_Comment by @zanieb on 2024-10-25 13:39_

Hm, this shouldn't happen. We should check the URL-level credential cache first, then, only if a request fails, should we check the realm-level credential cache. 

Can you share `RUST_LOG=uv=trace` logs? Be sure to redact your credentials.


---

_Assigned to @zanieb by @zanieb on 2024-10-25 13:39_

---

_Comment by @hbeukers on 2024-10-25 14:04_

```
DEBUG uv 0.4.26
DEBUG Found project root: `/builds/myteam/mypackage`
DEBUG No workspace root found, using project root
DEBUG Discovered project `mypackage` at: /builds/myteam/mypackage
DEBUG Reading requests from `/builds/myteam/mypackage/.python-version`
DEBUG Searching for Python 3.11 in managed installations or system path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ldd --version`: "ld.so (Debian GLIBC 2.36-9+deb12u8) stable release version 2.36.\nCopyright (C) 2022 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.36 in stdout of `ldd --version`
TRACE Searching PATH for executables: python, python3, python3.11
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Querying interpreter executable at /usr/local/bin/python
DEBUG Found `cpython-3.11.10-linux-x86_64-gnu` at `/usr/local/bin/python` (search path)
Using CPython 3.11.10 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
TRACE Caching credentials for https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple
TRACE Caching credentials for https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: mypackage @ file:///builds/myteam/mypackage
DEBUG No workspace root found, using project root
TRACE Performing lookahead for mypackage @ file:///builds/myteam/mypackage
DEBUG Solving with installed Python version: 3.11.10
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: mypackage*
DEBUG Searching for a compatible version of mypackage @ file:///builds/myteam/mypackage (*)
DEBUG Adding transitive dependency for mypackage==0.1.0: secondpackage>=1.0.0, <1.0.dev0
DEBUG Adding transitive dependency for mypackage==0.1.0: firstpackage>=1.0.0, <1.0.dev0
TRACE Fetching metadata for secondpackage from https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/
TRACE Fetching metadata for firstpackage from https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/70a791461bf74635/secondpackage.rkyv
DEBUG No cache entry for: https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/
TRACE Sending fresh GET request for https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/
TRACE Handling request for https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/
TRACE Request for https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/
TRACE Request for https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/ is fully authenticated
TRACE No cache entry exists for /root/.cache/uv/simple-v13/index/b83c37047cbdf300/firstpackage.rkyv
DEBUG No cache entry for: https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/
TRACE Sending fresh GET request for https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/
TRACE Handling request for https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/
TRACE Request for https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/
TRACE Request for https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/ is fully authenticated
TRACE cached request https://gitlab.mycompany.com/api/v4/projects/6172/packages/pypi/simple/secondpackage/ is storable because this is a shared cache and its response has a 'private' cache-control directive
TRACE Received package metadata for: secondpackage
DEBUG Searching for a compatible version of secondpackage (>=1.0.0, <1.1.dev0)
TRACE Selecting candidate for secondpackage with range >=1.0.0, <1.1.dev0 with 7 remote versions
TRACE found candidate for package PackageName("secondpackage") with range Range { segments: [(Included("1.0.0"), Excluded("1.1.dev0"))] } after 1 steps: "1.0.0" version
DEBUG Selecting: secondpackage==1.0.0 [compatible] (secondpackage-1.0.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/70a791461bf74635/secondpackage/secondpackage-1.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Sending fresh HEAD request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Handling request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Attempting unauthenticated request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE cached request https://gitlab.mycompany.com/api/v4/projects/2177/packages/pypi/simple/firstpackage/ is storable because this is a shared cache and its response has a 'private' cache-control directive
TRACE Received package metadata for: firstpackage
TRACE Request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://gitlab.mycompany.com
TRACE Retrying request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 with credentials from cache Credentials { username: Username(Some("firstindex-token")), password: Some("[MASKED]") }
WARN Range requests not supported for secondpackage-1.0.0-py3-none-any.whl; streaming wheel
TRACE No cache entry exists for /root/.cache/uv/wheels-v2/index/70a791461bf74635/secondpackage/secondpackage-1.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Sending fresh GET request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Handling request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Attempting unauthenticated request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
TRACE Request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://gitlab.mycompany.com
TRACE Retrying request for https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804 with credentials from cache Credentials { username: Username(Some("firstindex-token")), password: Some("[MASKED]") }
error: Failed to download `secondpackage==1.0.0`
  Caused by: HTTP status client error (404 Not Found) for url (https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804)
```

---

_Comment by @zanieb on 2024-10-25 14:20_

So you can see a log

```
No credentials in cache for URL https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/d1f9d072c85428dc20d5813c9cecdcedd593284573474353487598f60d9804/secondpackage-1.0.0-py3-none-any.whl#sha256=d1f9d072c85428dc20d5813c4285782457837502540d4ac3b18033c8f60d9804
```

The problem is that the package URL is not a subset of the index URL `https://gitlab.mycompany.com/api/v4/projects/6122/packages/pypi/simple` so we fall back to the realm cache which contains the credentials for your other index.

The authentication middleware requires a strict URL prefix match to use URL-level credentials. This is important for cases like GitHub repositories where  `https://github.com/astral-sh/uv` and `https://github.com/astral-sh/ruff` need different credentials. The solution here is to special case credential handling for index URLs #4583 in the URL cache to allow credentials for `https://gitlab.mycompany.com/api/v4/projects/6122/packages/pypi/simple` to be used for `https://gitlab.company.com/api/v4/projects/6122/packages/pypi/files/...`

---

_Label `bug` added by @zanieb on 2024-10-25 14:21_

---

_Comment by @hbeukers on 2024-10-25 15:05_

Thank you for the fast responses. Then we'll wait for #4583 to be implemented and, in the meantime, use a token that authorizes for both indexes at once.

---

_Referenced in [astral-sh/uv#9741](../../astral-sh/uv/issues/9741.md) on 2024-12-09 22:12_

---

_Referenced in [astral-sh/uv#11074](../../astral-sh/uv/pulls/11074.md) on 2025-01-29 20:55_

---

_Closed by @zanieb on 2025-01-30 16:22_

---

_Closed by @zanieb on 2025-01-30 16:22_

---

_Referenced in [astral-sh/uv#11244](../../astral-sh/uv/issues/11244.md) on 2025-02-05 15:15_

---
