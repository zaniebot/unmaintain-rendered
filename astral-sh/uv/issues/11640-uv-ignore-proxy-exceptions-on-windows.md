```yaml
number: 11640
title: uv ignore proxy exceptions on Windows
type: issue
state: closed
author: Fizcko
labels:
  - bug
  - external
assignees: []
created_at: 2025-02-19T20:57:58Z
updated_at: 2025-03-17T18:45:38Z
url: https://github.com/astral-sh/uv/issues/11640
synced_at: 2026-01-12T16:00:42Z
```

# uv ignore proxy exceptions on Windows

---

_@Fizcko_

### Summary

On Windows, in a corporate device the proxy exceptions defined in the proxy settings are ignored by uv.

Steps to reproduce
1. With proxy **enabled** on OS settings

Proxy config
```
ip: 10.0.10.250
port: 8080
exceptions: *.myorg.local
```
uv config file `%APPDATA%\uv\uv.toml` to use custom index url:
```
[[index]]
url = "https://artifactory.myorg.local/simple"
default = true
```
Run uv command
```
> uv add flask
⠇ test-python==0.1.0                                                                                                                                        error: Failed to fetch: `https://artifactory.myorg.local/simple/flask/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
>
```
Same command with verbose
```
> $env:RUST_LOG="uv=trace"
> uv add flask
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `X:\Temp\test_python`
DEBUG No workspace root found, using project root
TRACE Checking lock for `X:\Temp\test_python` at `C:\Users\johndoe\AppData\Local\Temp\uv-b254de633360c559.lock`
DEBUG Acquired lock for `X:\Temp\test_python`
DEBUG Reading Python requests from version file at `X:\Temp\test_python\.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.11.4, skipping probing: .venv\Scripts\python.exe
DEBUG The virtual environment's Python version satisfies `3.11`
DEBUG Released lock at `C:\Users\johndoe\AppData\Local\Temp\uv-b254de633360c559.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-python @ file:///X:/Temp/test_python
DEBUG No workspace root found, using project root
TRACE Performing lookahead for test-python @ file:///X:/Temp/test_python
DEBUG Solving with installed Python version: 3.11.4
DEBUG Solving with target Python version: >=3.11
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: test-python*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: test-python. remaining choices:
DEBUG Searching for a compatible version of test-python @ file:///X:/Temp/test_python (*)
DEBUG Adding direct dependency: flask*
TRACE Fetching metadata for flask from https://artifactory.myorg.local/simple/flask/
TRACE Assigned packages: root==0a0.dev0, test-python==0.1.0
TRACE Chose package for decision: flask. remaining choices:
TRACE Checking lock for `C:\Users\johndoe\AppData\Local\uv\cache\simple-v15\index\e03af8d8960669e8\flask.lock` at `C:\Users\johndoe\AppData\Local\uv\cache\simple-v15\index\e03af8d8960669e8\flask.lock`
DEBUG Acquired lock for `C:\Users\johndoe\AppData\Local\uv\cache\simple-v15\index\e03af8d8960669e8\flask.lock`
TRACE No cache entry exists for C:\Users\johndoe\AppData\Local\uv\cache\simple-v15\index\e03af8d8960669e8\flask.rkyv
DEBUG No cache entry for: https://artifactory.myorg.local/simple/flask/
TRACE Sending fresh GET request for https://artifactory.myorg.local/simple/flask/
TRACE Handling request for https://artifactory.myorg.local/simple/flask/
TRACE Request for https://artifactory.myorg.local/simple/flask/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myorg.local/simple/flask/
TRACE Attempting unauthenticated request for https://artifactory.myorg.local/simple/flask/
DEBUG Transient request failure for https://artifactory.myorg.local/simple/flask/, retrying: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
TRACE Handling request for https://artifactory.myorg.local/simple/flask/
TRACE Request for https://artifactory.myorg.local/simple/flask/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myorg.local/simple/flask/
TRACE Attempting unauthenticated request for https://artifactory.myorg.local/simple/flask/
DEBUG Transient request failure for https://artifactory.myorg.local/simple/flask/, retrying: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
TRACE Handling request for https://artifactory.myorg.local/simple/flask/
TRACE Request for https://artifactory.myorg.local/simple/flask/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myorg.local/simple/flask/
TRACE Attempting unauthenticated request for https://artifactory.myorg.local/simple/flask/
DEBUG Transient request failure for https://artifactory.myorg.local/simple/flask/, retrying: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
TRACE Handling request for https://artifactory.myorg.local/simple/flask/
TRACE Request for https://artifactory.myorg.local/simple/flask/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myorg.local/simple/flask/
TRACE Attempting unauthenticated request for https://artifactory.myorg.local/simple/flask/
DEBUG Transient request failure for https://artifactory.myorg.local/simple/flask/, retrying: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("artifactory.myorg.local")), port: None, path: "/simple/flask/", query: None, fragment: None }, WrappedReqwestError(Middleware(Request failed after 3 retries

Caused by:
    0: error sending request for url (https://artifactory.myorg.local/simple/flask/)
    1: client error (Connect)
    2: proxy authentication required))) }
TRACE Cannot retry error: not an IO error
DEBUG Released lock at `C:\Users\johndoe\AppData\Local\uv\cache\simple-v15\index\e03af8d8960669e8\flask.lock`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
error: Failed to fetch: `https://artifactory.myorg.local/simple/flask/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://artifactory.myorg.local/simple/flask/)
  Caused by: client error (Connect)
  Caused by: proxy authentication required
```

2. With proxy **disabled** on OS settings

```
> uv add flask
Resolved 9 packages in 213ms
Prepared 8 packages in 342ms
░░░░░░░░░░░░░░░░░░░░ [0/8] Installing wheels...                                                                                                             warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 8 packages in 52ms
 + blinker==1.9.0
 + click==8.1.8
 + colorama==0.4.6
 + flask==3.1.0
 + itsdangerous==2.2.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + werkzeug==3.1.3
```

### Platform

Windows 10 & 11

### Version

uv 0.6.2

### Python version

python 3.11

---

_Label `bug` added by @Fizcko on 2025-02-19 20:57_

---

_Comment by @zanieb on 2025-02-19 21:11_

Thanks for adding more details! I appreciate it.

How does the proxy configuration work? Where do you do that?

We don't read proxy setting directly, so we'll need to track support for this upstream in `reqwest` which implements the network stack we use.

---

_Label `external` added by @zanieb on 2025-02-19 21:12_

---

_Comment by @Fizcko on 2025-02-19 21:25_

The proxy settings are directly set into windows

![Image](https://github.com/user-attachments/assets/35f2f4ee-ab5f-48a1-ac70-647d2f5df021)

![Image](https://github.com/user-attachments/assets/bdc50083-b63c-4ee0-86eb-da3563dc0cee)

You can simulate one with [CCProxy](https://www.youngzsoft.net/ccproxy/proxy-server-download.htm) software


---

_Comment by @Fizcko on 2025-02-19 21:33_

The linux equivalent would be:

```
export HTTP_PROXY="http://10.0.10.250:8080"
export HTTPS_PROXY="http://10.0.10.250:8080"
export NO_PROXY="*.myorg.local"
```

But I have not tested under Linux if the behavior was the same.

---

_Comment by @zanieb on 2025-02-19 21:35_

Thanks again!

I think this is the same as https://github.com/seanmonstar/reqwest/issues/1444

Can you confirm that registry key is the relevant place this is stored?

---

_Comment by @Fizcko on 2025-02-19 21:41_

Yes it is

![Image](https://github.com/user-attachments/assets/54c58073-b336-43b2-838a-37a91c564a38)

---

_Comment by @zanieb on 2025-02-19 21:47_

Thanks! I'll follow-up there and see if we can get this fixed.

---

_Comment by @rv2931 on 2025-03-04 16:02_

Hi. I meet this issue too and this ticket confirm it is a bug/lack... It is important to know that proxy configuration is not only http_proxy/https_proxy, but no_proxy too and overall it is really important when you need it
Hope it will be fixed quite soon
Thanks to community

---

_Comment by @zanieb on 2025-03-05 18:55_

We're just waiting for the fix to be released upstream.

---

_Comment by @rv2931 on 2025-03-06 10:56_

Okay. I was wondering about the impact of these bug, but for the moment blocking problem as I can't test or deploy anything on my deployement server as it can't access to internal ressource of my company (python package from internal gitlab instance) as UV pip try to get them from public internet
I will try to workaround the problem by installing our internal package via native pip that's work correctly, maybe filling the cache before UV sync and maybe UV will just use the cache ?

---

_Comment by @Fizcko on 2025-03-06 13:26_

A [fix](https://github.com/seanmonstar/reqwest/pull/2559) have been push and merged.

As @zanieb said we are waiting for a new release of [reqwest](https://github.com/seanmonstar/reqwest/releases) (above than `v0.12.12`).

@rv2931 as workaround (for now) you can use this:

```
exceptions: *.myorg.local;*.myneworg.local
```

Run this in a powershell command with comma separator and without `*.`
```
$env:no_proxy="myorg.local,myneworg.local"
```

---

_Comment by @rv2931 on 2025-03-06 14:29_

Ok but I don't understand your workaround as it is the topic if the bug itself: no_proxy is not managed correctly by uv commands
So in addition I have the same problem on Linux during docker build based on Ubuntu: 22.04.
Installation with native pip is ok but not with iv sync "could 'ot resolve" so it is not linked specifically to windows version
I'm trying a workaround by unsettling http_proxy/https_proxy and using the `uv sync --package <mylocal_gitlab_repository>` then resetting the proxybbalue after for the other public packages

---

_Comment by @zanieb on 2025-03-06 23:39_

@rv2931 sounds like another issue? maybe https://github.com/astral-sh/uv/issues/8450? This issue is specifically about Windows, hence the confusion.

---

_Comment by @JimmyMtl on 2025-03-17 13:58_

Hi, Reqwest seems to have made a new release that contains the fix, see https://github.com/seanmonstar/reqwest/releases/tag/v0.12.13. And update of the library has been made on uv's side this morning, see https://github.com/astral-sh/uv/commit/fe06f1a7ce69087d30f4a5104cd6c5fbee235dc4. 
I think that the next step is to wait for a new uv release ?

---

_Comment by @charliermarsh on 2025-03-17 13:59_

Yeah, we'll probably release today or tomorrow.

---

_Comment by @zanieb on 2025-03-17 18:45_

Closing this since it's merged

---

_Closed by @zanieb on 2025-03-17 18:45_

---
