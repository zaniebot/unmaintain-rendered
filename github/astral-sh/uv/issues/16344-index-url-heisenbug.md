---
number: 16344
title: Index URL heisenbug
type: issue
state: closed
author: zacps
labels:
  - bug
assignees: []
created_at: 2025-10-17T13:30:30Z
updated_at: 2025-10-23T10:14:13Z
url: https://github.com/astral-sh/uv/issues/16344
synced_at: 2026-01-07T13:12:19-06:00
---

# Index URL heisenbug

---

_Issue opened by @zacps on 2025-10-17 13:30_

### Summary

I have an extremely weird issue with custom index URLs.

In one project I have a couple of deps pulled from gitlab:

```toml
[tool.uv.sources]
mypackage1 = { index = "index1" }
mypackage2 = { index = "index2" }

[[tool.uv.index]]
name = "mypackage1"
url = "https://our_current_host/api/v4/projects/1/packages/pypi/simple"
explicit = true
authenticate = "always"

[[tool.uv.index]]
name = "mypackage2"
url = "https://our_current_host/api/v4/projects/2/packages/pypi/simple"
explicit = true
authenticate = "always"
```

Because we're moving from `our_current_host` to `our_current_host2` I updated the URLs in `pyproject.toml`, and replaced them in the lock.

However, uv still attempts to fetch from the old URL, despite it not being present in `pyproject.toml`, `uv.lock`, or environment variables. Even running `uv cache clean && rm -rf .venv` doesn't help.

I honestly have no idea where uv could even be finding the old URL.

### Platform

Ubuntu 24.04

### Version

0.9.2

### Python version

3.11.5

---

_Label `bug` added by @zacps on 2025-10-17 13:30_

---

_Closed by @zacps on 2025-10-17 13:33_

---

_Reopened by @zacps on 2025-10-17 13:37_

---

_Comment by @konstin on 2025-10-17 13:40_

> Because we're moving from `our_current_host` to `our_current_host2` I updated the URLs in `pyproject.toml`, and replaced them in the lock.

Updating the lockfile manually is not supported, you need to use `uv lock` to update the lockfile.

---

_Comment by @zacps on 2025-10-17 13:54_

`uv lock` didn't correctly update the lockfile in this situation. To make matters worse, once in this state I have no idea where uv is even storing the old url state to fix the problem.

This also occurs when using uv pip directly, for example:

```
$ uv pip install mypackage --index-url https://our_current_host2/api/v4/projects/1/packages/pypi/simple
⠹ mypackage==0.2.3                                                                                                                                        error: Failed to fetch: `https://our_current_host/api/v4/projects/1/packages/pypi/files/HASH/mypackage-0.2.3-py3-none-any.whl`
  Caused by: HTTP status client error (401 Unauthorized) for url (https://our_current_host/api/v4/projects/12/packages/pypi/files/HASH/mypackage-0.2.3-py3-none-any.whl)
```

---

_Comment by @konstin on 2025-10-17 15:40_

Can you run with `-v`? This can helps finding where the old URL comes from.

---

_Comment by @zacps on 2025-10-20 11:22_

I ran with many v's, here are the excepts I think are most likely to be useful.


Run start, includes credential fetching for `new-host`:
```log
DEBUG uv 0.9.3
TRACE Checking shared lock for `/home/zac/.cache/uv` at `/home/zac/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/zac/.cache/uv`
DEBUG Found project root: `/home/zac/company/project`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/zac/company/project` at `/tmp/uv-2409d5a4cadf6633.lock`
DEBUG Acquired lock for `/home/zac/company/project`
DEBUG Reading Python requests from version file at `/home/zac/company/project/.python-version`
DEBUG Using Python request `3.11.5` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
TRACE Found cached interpreter info for Python 3.11.5, skipping query of: .venv/bin/python3
DEBUG The project environment's Python version satisfies the request: `Python 3.11.5`
TRACE The project environment's Python version meets the Python requirement: `==3.11.5`
TRACE The virtual environment's Python interpreter meets the Python preference: `prefer managed`
DEBUG Released lock at `/tmp/uv-2409d5a4cadf6633.lock`
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
TRACE Caching credentials for https://new-host/api/v4/projects/12/packages/pypi
TRACE Caching credentials for https://new-host/api/v4/projects/12/packages/pypi/simple
TRACE Caching credentials for https://new-host/api/v4/projects/47/packages/pypi
TRACE Caching credentials for https://new-host/api/v4/projects/47/packages/pypi/simple
TRACE Read credentials for index mypackage2
TRACE Caching credentials for https://new-host/api/v4/projects/47/packages/pypi
TRACE Caching credentials for https://new-host/api/v4/projects/47/packages/pypi/simple
TRACE Read credentials for index mypackage1
TRACE Caching credentials for https://new-host/api/v4/projects/12/packages/pypi
TRACE Caching credentials for https://new-host/api/v4/projects/12/packages/pypi/simple
DEBUG Using request timeout of 180s
DEBUG Found static `pyproject.toml` for: project @ file:///home/zac/company/project
DEBUG No workspace root found, using project root
TRACE Caching credentials for https://new-host/api/v4/projects/47/packages/pypi/simple
TRACE Caching credentials for https://new-host/api/v4/projects/12/packages/pypi/simple
DEBUG Resolving despite existing lockfile due to mismatched dependency groups for: `project==0.1.0`
```

Connection to `new-host` to fetch metadata:
```
TRACE Response from https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ is storable because this is a shared cache and has a 'private' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 0ns
TRACE Request to https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ has a cached response that does not permit staleness because the response has a 'must-revalidate' cache-control directive set
TRACE Request to https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ does not have a fresh cache entry because its age is 11 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/
DEBUG Sending revalidation request for: https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/
TRACE Handling request for https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ with authentication policy always
TRACE Request for https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ is unauthenticated, checking cache
TRACE Found cached credentials for URL https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/
TRACE Request for https://new-host/api/v4/projects/12/packages/pypi/simple/mypackage1/ is fully authenticated
TRACE checkout waiting for idle connection: ("https", new-host)
DEBUG starting new connection: https://new-host/
TRACE Http::connect; scheme=Some("https"), host=Some("new-host"), port=None
```

Connecting to old-host for some reason (remember, this is just after a cache clean):
```
TRACE No cache entry exists for /home/zac/.cache/uv/wheels-v5/index/d84fc819ead3434e/mypackage1/0.2.2-py3-none-any.msgpack
DEBUG No cache entry for: https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE Sending fresh HEAD request for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE Handling request for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE checkout waiting for idle connection: ("https", old-host)
DEBUG starting new connection: https://old-host/
```

Finally failing to authenticate against `old-host`:
```
TRACE Request for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://old-host
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `/home/zac/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Acquired lock for `credentials store`
DEBUG Loaded credential file /home/zac/.local/share/uv/credentials/credentials.toml
DEBUG Released lock at `/home/zac/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Checking text store for credentials for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE drop_stream_ref; stream=Stream { id: StreamId(1), state: Closed(EndStream), is_counted: false, ref_count: 1, send_flow: FlowControl { window_size: Window(65535), available: Window(0) }, requested_send_capacity: 0, buffered_send_data: 0, recv_flow: FlowControl { window_size: Window(2097152), available: Window(2097152) }, in_flight_recv_data: 0, content_length: Head }
TRACE transition_after; stream=StreamId(1); state=Closed(EndStream); is_closed=true; pending_send_empty=true; buffered_send_data=0; num_recv=0; num_send=0
TRACE Considering retry of response HTTP 401 Unauthorized for https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl
TRACE Cannot retry nested reqwest middleware error
DEBUG Released lock at `/home/zac/company/project/.venv/.lock`
DEBUG Released lock at `/home/zac/.cache/uv/.lock`
TRACE Streams::recv_eof
TRACE Streams::recv_eof
TRACE Error trace: Failed to fetch: `https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl`

Caused by:
    HTTP status client error (401 Unauthorized) for url (https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl)
error: Failed to fetch: `https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl`
  Caused by: HTTP status client error (401 Unauthorized) for url (https://old-host/api/v4/projects/12/packages/pypi/files/1bd9e0d588c66310eb25a28f38ff97f1a6367aeba214b8002e1a72c130d039f3/mypackage1-0.2.2-py3-none-any.whl)
```

---

_Comment by @konstin on 2025-10-22 11:54_

It looks like we're using new-url for the simple index page, and then old-url for the files themselves. Could you check the simple index page e.g. in a browser and confirm that the file links in the HTML or JSON links to new-url, not old-url?

---

_Comment by @zacps on 2025-10-23 10:14_

Ahh yes I think you're right, we're migrating to the new host and the old one is still used (and hence where the generated links point to).

Sorry for the false report!

---

_Closed by @zacps on 2025-10-23 10:14_

---
