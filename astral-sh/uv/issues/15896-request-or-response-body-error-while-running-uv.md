```yaml
number: 15896
title: request or response body error while running uv add
type: issue
state: open
author: ajrup
labels:
  - bug
  - needs-mre
  - external
assignees: []
created_at: 2025-09-16T16:43:39Z
updated_at: 2025-09-29T11:07:06Z
url: https://github.com/astral-sh/uv/issues/15896
synced_at: 2026-01-10T03:23:54Z
```

# request or response body error while running uv add

---

_Issue opened by @ajrup on 2025-09-16 16:43_

### Summary

I am trying to migrate my team to UV, however am facing some integration issues with our Artifactory instance that I do not experience with pip or poetry. It appears to be a TLS issue or something to do with retries. I've looked through a number of similar issues and am using the latest version of uv, but have had no luck in resolving this error.

## Setup

`pyproject.toml`:
```toml
[project]
name = "test-dir"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = []

[[tool.uv.index]]
name = "artifactory"
url = "https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/"
default = true
```

## Reproduce

```
uv add requests --verbose
```

## Logs

```
PS C:\my_workspace\test_dir> uv add requests --verbose
DEBUG uv 0.8.17
DEBUG Found project root: `C:\my_workspace\test_dir`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\my_workspace\test_dir`
DEBUG Reading Python requests from version file at `C:\my_workspace\test_dir\.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.10`
DEBUG Released lock at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-dir @ file:///C:/my_workspace/test_dir
DEBUG No workspace root found, using project root
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `test-dir==0.1.0`
  Requested: {Requirement { name: PackageName("requests"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
  Existing: {}
DEBUG Solving with installed Python version: 3.10.2
DEBUG Solving with target Python version: >=3.10
DEBUG Adding direct dependency: test-dir*
DEBUG Searching for a compatible version of test-dir @ file:///C:/my_workspace/test_dir (*)
DEBUG Adding direct dependency: requests*
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\requests.lock`
DEBUG Found stale response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Sending revalidation request for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Found modified response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
WARN Skipping file for requests: requests-2.23.0-py2.7.egg
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\requests.lock`
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.5 [compatible] (requests-2.32.5-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\requests\requests-2.32.5-py3-none-any.lock`
DEBUG Found fresh response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\requests\requests-2.32.5-py3-none-any.lock`
DEBUG Adding transitive dependency for requests==2.32.5: certifi>=2017.4.17
DEBUG Adding transitive dependency for requests==2.32.5: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.5: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.5: urllib3>=1.21.1, <3
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\certifi.lock`
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\charset-normalizer.lock`
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\idna.lock`
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\urllib3.lock`
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/
DEBUG Found stale response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/certifi/
DEBUG Sending revalidation request for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/certifi/
DEBUG Found stale response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/urllib3/
DEBUG Sending revalidation request for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/urllib3/
DEBUG Found stale response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/idna/
DEBUG Sending revalidation request for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/idna/
DEBUG Found modified response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/idna/
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\idna.lock`
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\idna\idna-3.10-py3-none-any.lock`
DEBUG Found modified response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/certifi/
DEBUG Found fresh response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\idna\idna-3.10-py3-none-any.lock`
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\certifi.lock`
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2025.8.3 [compatible] (certifi-2025.8.3-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\certifi\certifi-2025.8.3-py3-none-any.lock`
DEBUG Found modified response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/urllib3/
DEBUG Found fresh response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/e5/48/1549795ba7742c948d2ad169c1c8cdbae65bc450d6cd753d124b17c8cd32/certifi-2025.8.3-py3-none-any.whl
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\certifi\certifi-2025.8.3-py3-none-any.lock`
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\urllib3.lock`
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\urllib3\urllib3-2.5.0-py3-none-any.lock`
DEBUG Found fresh response for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\wheels-v5\index\6c26be7dda6540b7\urllib3\urllib3-2.5.0-py3-none-any.lock`
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\6c26be7dda6540b7\charset-normalizer.lock`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
DEBUG Released lock at `C:\my_workspace\test_dir\.venv\.lock`
error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/`
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

### Platform

Windows 11 Enterprise x86_64

### Version

uv 0.8.17

### Python version

Python 3.10.2

---

_Label `bug` added by @ajrup on 2025-09-16 16:43_

---

_Comment by @konstin on 2025-09-16 17:15_

Can you access the registry with `curl`? Does your system have any specific proxy or similar settings?

---

_Comment by @ajrup on 2025-09-16 18:25_

The Artifactory instance bypasses the proxy. Being explicit about it, this command still fails:

```
$ HTTP_PROXY="" HTTPS_PROXY="" uv add requests
⠼ certifi==2025.8.3
  error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/charset-normalizer/`
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

Here is a curl command for an importlib wheel that succeeds:

```
$ curl https://my_artifactory.com/artifactory/pypi-virtual/db/2a/728c8ae66011600fac5731a7db030d23c42f1321fd9547654f0c3b2b32d7/importlib_resources-6.4.4-py3-none-any.whl -o importlib.whl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 35608  100 35608    0     0   330k      0 --:--:-- --:--:-- --:--:--  344k
```

However, here is a curl command that fails:
```
$ curl https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ -o curl_output.xml
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 61729    0 61729    0     0   883k      0 --:--:-- --:--:-- --:--:--  941k
curl: (56) Failure when receiving data from the peer
```

The xml does look well formed when I inspect it:
```xml
<!DOCTYPE html>
<html><head><title>Simple Index</title><meta name="api-version" value="2" /></head><body>
<a href="../../packages/packages/62/35/0230421b8c4efad6624518028163329ad0c2df9e58e6b3bee013427bf8f6/requests-0.10.0.tar.gz#sha256=210a82e678c45d433a4ad1f105974b3102a8ab5198872dc0a3238a8750d4c65e" rel="internal">requests-0.10.0.tar.gz</a><br />
<a href="../../packages/packages/b4/56/ba2d803383ec32d70f8faa7df5eb37ee9b3fc662ff68b7ab01ad9740b83a/requests-0.10.1.tar.gz#sha256=da6031575a30c7b65ea99465183468349b3645e6bf5322e49d53f565b27ed2b5" rel="internal">requests-0.10.1.tar.gz</a><br />
<!-- more -->
<a href="../../packages/packages/64/20/2133a092a0e87d1c250fe48704974b73a1341b7e4f800edecf40462a825d/requests-2.9.2.tar.gz#sha256=d8be941a08cf36e4f424ac76073eb911e5e646a33fcb3402e1642c426bf34682" rel="internal">requests-2.9.2.tar.gz</a><br />
</body></html>
```


Running the curl command with verbose output yields this:
```
$ curl https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ -o curl_output.xml --verbose
* processing: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 45.54.188.191:443...
* Connected to my_artifactory.com port 443
* schannel: disabled automatic use of client certificate
* using HTTP/1.x
> GET /artifactory/api/pypi/pypi-virtual/simple/requests/ HTTP/1.1
> Host: my_artifactory.com
> User-Agent: curl/8.2.1
> Accept: */*
>
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* HTTP 1.0, assume close after body
< HTTP/1.0 200
< Date: Tue, 16 Sep 2025 18:14:28 GMT
< Server: Apache
< Some redacted info
< Content-Type: text/html
< Connection: close
<
{ [7904 bytes data]
* schannel: failed to decrypt data, need more data
{ [16345 bytes data]
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
{ [8184 bytes data]
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
{ [8184 bytes data]
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
{ [8184 bytes data]
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
{ [8184 bytes data]
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
* schannel: failed to decrypt data, need more data
{ [4744 bytes data]
* schannel: server closed abruptly (missing close_notify)
100 61729    0 61729    0     0   817k      0 --:--:-- --:--:-- --:--:--  861k
* Closing connection
* schannel: shutting down SSL/TLS connection with my_artifactory.com port 443
curl: (56) Failure when receiving data from the peer
```

It's just rather strange that `venv` and `pip` still work though, while `uv` does not:

```
PS C:\my_workspace> .\test_venv\Scripts\pip.exe install requests
Looking in indexes: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple
Collecting requests
  Downloading https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/1e/db/4254e3eabe8020b458f1a747140d32277ec7a271daf1d235b70dc0b4e6e3/requests-2.32.5-py3-none-any.whl (64 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 64.7/64.7 kB 3.4 MB/s eta 0:00:00
Collecting charset_normalizer<4,>=2 (from requests)
  Downloading https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/f4/9c/996a4a028222e7761a96634d1820de8a744ff4327a00ada9c8942033089b/charset_normalizer-3.4.3-cp311-cp311-win_amd64.whl (107 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 107.1/107.1 kB 6.1 MB/s eta 0:00:00
Collecting idna<4,>=2.5 (from requests)
  Using cached https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl (70 kB)
Collecting urllib3<3,>=1.21.1 (from requests)
  Using cached https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl (129 kB)
Collecting certifi>=2017.4.17 (from requests)
  Downloading https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/packages/packages/e5/48/1549795ba7742c948d2ad169c1c8cdbae65bc450d6cd753d124b17c8cd32/certifi-2025.8.3-py3-none-any.whl (161 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 161.2/161.2 kB 4.9 MB/s eta 0:00:00
Installing collected packages: urllib3, idna, charset_normalizer, certifi, requests
Successfully installed certifi-2025.8.3 charset_normalizer-3.4.3 idna-3.10 requests-2.32.5 urllib3-2.5.0

[notice] A new release of pip is available: 24.0 -> 25.2
[notice] To update, run: C:\my_workspace\test_venv\Scripts\python.exe -m pip install --upgrade pip
PS C:\my_workspace>
```

---

_Comment by @zanieb on 2025-09-16 18:34_

Does uv work with `--native-tls`?

---

_Comment by @ajrup on 2025-09-16 18:44_

It does not:

```
PS C:\my_workspace\test_dir> uv add --verbose --native-tls requests
DEBUG uv 0.8.17
DEBUG Found project root: `C:\my_workspace\test_dir`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\my_workspace\test_dir`
DEBUG Reading Python requests from version file at `C:\my_workspace\test_dir\.python-version`
DEBUG Using Python request `3.10` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.10`
DEBUG Released lock at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-dir @ file:///C:/my_workspace/test_dir
DEBUG No workspace root found, using project root
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `test-dir==0.1.0`
  Requested: {Requirement { name: PackageName("requests"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
  Existing: {}
DEBUG Solving with installed Python version: 3.10.2
DEBUG Solving with target Python version: >=3.10
DEBUG Adding direct dependency: test-dir*
DEBUG Searching for a compatible version of test-dir @ file:///C:/my_workspace/test_dir (*)
DEBUG Adding direct dependency: requests*
DEBUG Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
DEBUG No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
DEBUG Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
DEBUG Released lock at `C:\my_workspace\test_dir\.venv\.lock`
error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
PS C:\my_workspace\test_dir>
```

---

_Comment by @zanieb on 2025-09-16 18:48_

Alas (it seemed like sort of a long-shot, but that's a difference between uv and pip).

If curl does not succeed consistently, I'm not sure what we can do here.

It's not a huge surprise that the behavior from pip is different, as we have an entirely different stack implementing HTTPS.

---

_Comment by @ajrup on 2025-09-16 19:01_

Gotcha. I guess it's not clear to me exactly where the issue is. Is this a uv issue, or an issue with my network, a certificate issue, etc? I am more than willing to dig in to it a bit, but just need some direction on where the issue is at. Would strongly like to migrate to uv if I can get it working. Or if there's any sort of workaround that I can temporarily use while debugging?

---

_Comment by @zanieb on 2025-09-16 19:14_

I'd suggest taking the curl reproduction and raising it with Artifactory, as that's an isolated example that indicates they are probably doing something wrong.

If you set `RUST_LOG=trace UV_LOG_CONTEXT=1` then you'll get some verbose logs from the networking stack we depend on as well, which may be helpful.

---

_Comment by @ajrup on 2025-09-16 19:31_

Here's with the additional logging. If this looks ok, I will bring up the issue to Artifactory:

```
PS C:\my_workspace\test_dir> $env:RUST_LOG="trace"
PS C:\my_workspace\test_dir> $env:UV_LOG_CONTEXT="1"
PS C:\my_workspace\test_dir> uv add requests
    0.000047s DEBUG uv uv 0.8.17
    0.002351s DEBUG uv_workspace::workspace Found project root: `C:\my_workspace\test_dir`
    0.003050s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.003480s TRACE uv_fs Checking lock for `C:\my_workspace\test_dir` at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
    0.003594s DEBUG uv_fs Acquired lock for `C:\my_workspace\test_dir`
    0.004136s DEBUG uv_python::version_files Reading Python requests from version file at `C:\my_workspace\test_dir\.python-version`
    0.004258s DEBUG uv::commands::project Using Python request `3.10` from version file at `.python-version`
    0.004352s DEBUG uv_python::environment Checking for Python environment at: `.venv`
    0.006473s TRACE uv_python::interpreter Found cached interpreter info for Python 3.10.2, skipping query of: .venv\Scripts\python.exe
    0.006900s DEBUG uv::commands::project The project environment's Python version satisfies the request: `Python 3.10`
    0.007006s TRACE uv::commands::project The project environment's Python version meets the Python requirement: `>=3.10`
    0.007088s TRACE uv::commands::project The virtual environment's Python interpreter meets the Python preference: `prefer managed`
    0.007173s DEBUG uv_fs Released lock at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
    0.007585s TRACE uv_fs Checking lock for `.venv` at `.venv\.lock`
    0.007658s DEBUG uv_fs Acquired lock for `.venv`
 uv_requirements::specification::from_source source=Named(Requirement { name: PackageName("requests"), extras: [], version_or_url: None, marker: true, origin: None })
    0.011862s DEBUG uv_client::base_client Using request timeout of 30s
 uv_client::linehaul::linehaul
 uv_resolver::flat_index::from_entries
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=test-dir @ file:///C:/my_workspace/test_dir
    0.013687s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: test-dir @ file:///C:/my_workspace/test_dir
    0.014267s   1ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.014518s DEBUG uv::commands::project::lock Resolving despite existing lockfile due to mismatched requirements for: `test-dir==0.1.0`
   Requested: {Requirement { name: PackageName("requests"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
   Existing: {}
    0.016035s TRACE uv_requirements::lookahead Performing lookahead for test-dir @ file:///C:/my_workspace/test_dir
 uv_resolver::resolver::solve
    0.017469s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.10.2
    0.017545s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.10
    0.017697s   0ms TRACE uv_resolver::resolver Assigned packages:
    0.017799s   0ms TRACE uv_resolver::resolver Chose package for decision: root. remaining choices:
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.018110s   0ms DEBUG uv_resolver::resolver Adding direct dependency: test-dir*
    0.018348s   1ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
    0.018472s   1ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0
    0.018543s   1ms TRACE uv_resolver::resolver Chose package for decision: test-dir. remaining choices:
    0.018722s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of test-dir @ file:///C:/my_workspace/test_dir (*)
   uv_resolver::resolver::get_dependencies_forking package=test-dir, version=0.1.0
     uv_resolver::resolver::get_dependencies package=test-dir, version=0.1.0
    0.018973s   1ms DEBUG uv_resolver::resolver Adding direct dependency: requests*
    0.019066s   1ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
 uv_resolver::resolver::process_request request=Versions requests
    0.019217s   1ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0, test-dir==0.1.0
    0.019286s   2ms TRACE uv_resolver::resolver Chose package for decision: requests. remaining choices:
   uv_client::registry_client::package_metadata package=requests
      0.019436s   0ms TRACE uv_client::registry_client Fetching metadata for requests from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
    0.020207s TRACE uv_fs Checking lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock` at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
 uv_resolver::resolver::process_request request=Prefetch requests *
    0.020411s DEBUG uv_fs Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
     uv_client::cached_client::get_cacheable_with_retry
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            0.021080s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          0.021179s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            0.021358s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.021489s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            0.021573s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            0.021678s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.021763s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.021947s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            0.022045s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            0.022168s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            0.056103s  34ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            0.058547s  37ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            0.070235s  48ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            0.070416s  49ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            0.070582s  49ms TRACE hyper_util::client::legacy::client connection is ready
            0.070684s  49ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            0.278873s 257ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        0.283492s 262ms TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        0.283687s 263ms TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        0.283768s 263ms TRACE uv_client::base_client Cannot retry nested reqwest error
        0.283827s 263ms TRACE uv_client::base_client Retrying error: `unexpected end of file`
        0.283916s 263ms DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            1.268301s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          1.268413s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            1.268571s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            1.268661s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            1.268724s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            1.268792s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            1.268852s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            1.268949s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            1.269098s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            1.269245s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            1.269768s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            1.270792s   2ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            1.284720s  16ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            1.284860s  16ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            1.284961s  16ms TRACE hyper_util::client::legacy::client connection is ready
            1.285060s  16ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            1.452921s 184ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        1.457510s   1s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        1.457642s   1s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        1.457760s   1s  TRACE uv_client::base_client Cannot retry nested reqwest error
        1.457842s   1s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
        1.457914s   1s  DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            2.079644s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          2.079723s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            2.079862s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.079968s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            2.080069s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            2.080166s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.080231s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.080324s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            2.080405s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            2.080473s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            2.081022s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            2.082067s   2ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            2.096097s  16ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            2.096330s  16ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            2.096505s  16ms TRACE hyper_util::client::legacy::client connection is ready
            2.096586s  16ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            2.124518s  44ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        2.129426s   2s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        2.129593s   2s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        2.129721s   2s  TRACE uv_client::base_client Cannot retry nested reqwest error
        2.129792s   2s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
        2.129868s   2s  DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            2.968928s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          2.968995s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            2.969200s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.969278s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            2.969335s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            2.969390s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.969446s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.969584s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            2.969721s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            2.969827s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            2.970355s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            2.971331s   2ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            2.985541s  16ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            2.985824s  16ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            2.985998s  16ms TRACE hyper_util::client::legacy::client connection is ready
            2.986116s  17ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            3.019080s  50ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        3.023662s   3s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        3.023785s   3s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        3.023898s   3s  TRACE uv_client::base_client Cannot retry nested reqwest error
        3.023969s   3s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
      3.024054s   3s  DEBUG uv_fs Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
    3.024408s DEBUG uv::commands::project::add Reverting changes to `pyproject.toml`
    3.026897s DEBUG uv::commands::project::add Reverting changes to `uv.lock`
    3.028590s DEBUG uv_fs Released lock at `C:\my_workspace\test_dir\.venv\.lock`
    3.029114s TRACE uv Error trace: Request failed after 3 retries

 Caused by:
     0: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
     1: error decoding response body
     2: request or response body error
     3: error reading a body from connection
     4: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```


with --native-tls:
```
PS C:\my_workspace\test_dir> uv add requests --native-tls
    0.000029s DEBUG uv uv 0.8.17
    0.001562s DEBUG uv_workspace::workspace Found project root: `C:\my_workspace\test_dir`
    0.002063s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.002487s TRACE uv_fs Checking lock for `C:\my_workspace\test_dir` at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
    0.002595s DEBUG uv_fs Acquired lock for `C:\my_workspace\test_dir`
    0.003070s DEBUG uv_python::version_files Reading Python requests from version file at `C:\my_workspace\test_dir\.python-version`
    0.003200s DEBUG uv::commands::project Using Python request `3.10` from version file at `.python-version`
    0.003295s DEBUG uv_python::environment Checking for Python environment at: `.venv`
    0.005383s TRACE uv_python::interpreter Found cached interpreter info for Python 3.10.2, skipping query of: .venv\Scripts\python.exe
    0.005773s DEBUG uv::commands::project The project environment's Python version satisfies the request: `Python 3.10`
    0.005921s TRACE uv::commands::project The project environment's Python version meets the Python requirement: `>=3.10`
    0.006017s TRACE uv::commands::project The virtual environment's Python interpreter meets the Python preference: `prefer managed`
    0.006106s DEBUG uv_fs Released lock at `C:\Users\myself\AppData\Local\Temp\1\uv-f914cc735fe915dd.lock`
    0.006498s TRACE uv_fs Checking lock for `.venv` at `.venv\.lock`
    0.006574s DEBUG uv_fs Acquired lock for `.venv`
 uv_requirements::specification::from_source source=Named(Requirement { name: PackageName("requests"), extras: [], version_or_url: None, marker: true, origin: None })
    0.010740s DEBUG uv_client::base_client Using request timeout of 30s
 uv_client::linehaul::linehaul
 uv_resolver::flat_index::from_entries
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=test-dir @ file:///C:/my_workspace/test_dir
    0.037287s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: test-dir @ file:///C:/my_workspace/test_dir
    0.037893s   1ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.038102s DEBUG uv::commands::project::lock Resolving despite existing lockfile due to mismatched requirements for: `test-dir==0.1.0`
   Requested: {Requirement { name: PackageName("requests"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
   Existing: {}
    0.039464s TRACE uv_requirements::lookahead Performing lookahead for test-dir @ file:///C:/my_workspace/test_dir
 uv_resolver::resolver::solve
    0.040756s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.10.2
    0.040820s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.10
    0.040923s   0ms TRACE uv_resolver::resolver Assigned packages:
    0.040991s   0ms TRACE uv_resolver::resolver Chose package for decision: root. remaining choices:
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.041280s   0ms DEBUG uv_resolver::resolver Adding direct dependency: test-dir*
    0.041491s   0ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
    0.041585s   0ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0
    0.041674s   1ms TRACE uv_resolver::resolver Chose package for decision: test-dir. remaining choices:
    0.041943s   1ms DEBUG uv_resolver::resolver Searching for a compatible version of test-dir @ file:///C:/my_workspace/test_dir (*)
   uv_resolver::resolver::get_dependencies_forking package=test-dir, version=0.1.0
     uv_resolver::resolver::get_dependencies package=test-dir, version=0.1.0
    0.042172s   1ms DEBUG uv_resolver::resolver Adding direct dependency: requests*
    0.042253s   1ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
 uv_resolver::resolver::process_request request=Versions requests
    0.042413s   1ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0, test-dir==0.1.0
    0.042500s   1ms TRACE uv_resolver::resolver Chose package for decision: requests. remaining choices:
   uv_client::registry_client::package_metadata package=requests
      0.042675s   0ms TRACE uv_client::registry_client Fetching metadata for requests from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
    0.043392s TRACE uv_fs Checking lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock` at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
 uv_resolver::resolver::process_request request=Prefetch requests *
    0.043561s DEBUG uv_fs Acquired lock for `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
     uv_client::cached_client::get_cacheable_with_retry
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            0.044268s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          0.044342s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            0.044494s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.044602s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            0.044675s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            0.044769s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.044846s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.045012s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            0.045092s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            0.045190s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            0.074721s  30ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            0.076806s  32ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            0.086405s  41ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            0.086622s  42ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            0.087098s  42ms TRACE hyper_util::client::legacy::client connection is ready
            0.087232s  42ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            0.116677s  72ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        0.121379s  77ms TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        0.121533s  77ms TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        0.121622s  77ms TRACE uv_client::base_client Cannot retry nested reqwest error
        0.121697s  77ms TRACE uv_client::base_client Retrying error: `unexpected end of file`
        0.121806s  78ms DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            0.535968s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          0.536101s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            0.536232s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.536296s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            0.536349s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            0.536402s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.536457s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.536526s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            0.536590s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            0.536647s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            0.537144s   0ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            0.538084s   1ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            0.552615s  16ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            0.552762s  16ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            0.552875s  16ms TRACE hyper_util::client::legacy::client connection is ready
            0.552955s  16ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            0.577869s  41ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        0.582415s 538ms TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        0.582515s 538ms TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        0.582597s 538ms TRACE uv_client::base_client Cannot retry nested reqwest error
        0.582676s 538ms TRACE uv_client::base_client Retrying error: `unexpected end of file`
        0.582793s 539ms DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            2.424556s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          2.424636s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            2.424826s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.425014s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            2.425184s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            2.425361s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.425463s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.425646s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            2.425760s   1ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            2.425922s   1ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            2.426454s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            2.440612s  15ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            2.457947s  33ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            2.458114s  33ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            2.458252s  33ms TRACE hyper_util::client::legacy::client connection is ready
            2.458349s  33ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            2.487315s  62ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        2.491841s   2s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        2.491972s   2s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        2.492046s   2s  TRACE uv_client::base_client Cannot retry nested reqwest error
        2.492121s   2s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
        2.492186s   2s  DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
 uv_client::cached_client::from_path_sync path="C:\\Users\\myself\\AppData\\Local\\uv\\cache\\simple-v17\\index\\5b298b0f3b1364ab\\requests.rkyv"
            3.143015s   0ms TRACE uv_client::cached_client No cache entry exists for C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
          3.143117s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            3.143234s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            3.143303s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            3.143359s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            3.143421s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            3.143476s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            3.143550s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            3.143609s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            3.143712s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            3.144318s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            3.158369s  15ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            3.176198s  33ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            3.176407s  33ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            3.176558s  33ms TRACE hyper_util::client::legacy::client connection is ready
            3.176633s  33ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            3.204536s  61ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        3.209122s   3s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        3.209244s   3s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        3.209313s   3s  TRACE uv_client::base_client Cannot retry nested reqwest error
        3.209383s   3s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
      3.209466s   3s  DEBUG uv_fs Released lock at `C:\Users\myself\AppData\Local\uv\cache\simple-v17\index\5b298b0f3b1364ab\requests.lock`
    3.209798s DEBUG uv::commands::project::add Reverting changes to `pyproject.toml`
    3.211874s DEBUG uv::commands::project::add Reverting changes to `uv.lock`
    3.213657s DEBUG uv_fs Released lock at `C:\my_workspace\test_dir\.venv\.lock`
    3.214152s TRACE uv Error trace: Request failed after 3 retries

 Caused by:
     0: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
     1: error decoding response body
     2: request or response body error
     3: error reading a body from connection
     4: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

---

_Comment by @zanieb on 2025-09-16 20:10_

Yeah

> peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof

appears to be the key information still. The documentation there seems relevant. It seems like something is wrong with their server.

cc @seanmonstar if you're interested.



---

_Label `needs-mre` added by @konstin on 2025-09-17 10:19_

---

_Label `external` added by @konstin on 2025-09-17 10:19_

---

_Comment by @ajrup on 2025-09-17 14:20_

Ok, I went through and did some more testing with a linux host that I have access to. On this host, I can verify that `curl` works reliably:

```
curl https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ -o curl_output.xml
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 61729    0 61729    0     0   410k      0 --:--:-- --:--:-- --:--:--  412k
```

I still see the error from this host when using `uv` (again, `pip` and `poetry` still work here):

```
$ RUST_LOG=trace UV_LOG_CONTEXT=1 uv add requests --verbose
    0.000026s DEBUG uv uv 0.8.17
    0.000691s DEBUG uv_workspace::workspace Found project root: `/my_workspace/test_uv/test_dir`
    0.001073s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.001393s TRACE uv_fs Checking lock for `/my_workspace/test_uv/test_dir` at `/tmp/uv-3434ff86bce25a16.lock`
    0.001512s DEBUG uv_fs Acquired lock for `/my_workspace/test_uv/test_dir`
    0.001673s DEBUG uv_python::version_files Reading Python requests from version file at `/my_workspace/test_uv/test_dir/.python-version`
    0.001750s DEBUG uv::commands::project Using Python request `3.9` from version file at `.python-version`
    0.001787s DEBUG uv_python::environment Checking for Python environment at: `.venv`
    0.002313s TRACE uv_python::interpreter Found cached interpreter info for Python 3.9.21, skipping query of: .venv/bin/python3
    0.002370s DEBUG uv::commands::project The project environment's Python version satisfies the request: `Python 3.9`
    0.002396s TRACE uv::commands::project The project environment's Python version meets the Python requirement: `>=3.9`
    0.002408s TRACE uv::commands::project The virtual environment's Python interpreter meets the Python preference: `prefer managed`
    0.002444s DEBUG uv_fs Released lock at `/tmp/uv-3434ff86bce25a16.lock`
    0.002491s TRACE uv_fs Checking lock for `.venv` at `.venv/.lock`
    0.002507s DEBUG uv_fs Acquired lock for `.venv`
 uv_requirements::specification::from_source source=Named(Requirement { name: PackageName("requests"), extras: [], version_or_url: None, marker: true, origin: None })
    0.003731s DEBUG uv_client::base_client Using request timeout of 30s
 uv_client::linehaul::linehaul
 uv_resolver::flat_index::from_entries
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=test-dir @ file:///my_workspace/test_uv/test_dir
    0.004620s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: test-dir @ file:///my_workspace/test_uv/test_dir
    0.004758s   0ms DEBUG uv_workspace::workspace No workspace root found, using project root
    0.005142s TRACE uv_requirements::lookahead Performing lookahead for test-dir @ file:///my_workspace/test_uv/test_dir
 uv_resolver::resolver::solve
    0.006872s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.9.21
    0.006908s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.9
    0.006996s   0ms TRACE uv_resolver::resolver Assigned packages:
    0.007019s   0ms TRACE uv_resolver::resolver Chose package for decision: root. remaining choices:
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.007266s   0ms DEBUG uv_resolver::resolver Adding direct dependency: test-dir*
    0.007450s   0ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(0) @ 0a0.dev0 without checking dependencies
    0.007502s   0ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0
    0.007514s   0ms TRACE uv_resolver::resolver Chose package for decision: test-dir. remaining choices:
    0.007537s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of test-dir @ file:///my_workspace/test_uv/test_dir (*)
   uv_resolver::resolver::get_dependencies_forking package=test-dir, version=0.1.0
     uv_resolver::resolver::get_dependencies package=test-dir, version=0.1.0
    0.007645s   0ms DEBUG uv_resolver::resolver Adding direct dependency: requests*
    0.007694s   0ms  INFO pubgrub::internal::partial_solution add_decision: Id::<PubGrubPackage>(1) @ 0.1.0 without checking dependencies
 uv_resolver::resolver::process_request request=Versions requests
    0.007731s   0ms TRACE uv_resolver::resolver Assigned packages: root==0a0.dev0, test-dir==0.1.0
    0.007759s   1ms TRACE uv_resolver::resolver Chose package for decision: requests. remaining choices:
   uv_client::registry_client::package_metadata package=requests
      0.007816s   0ms TRACE uv_client::registry_client Fetching metadata for requests from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
     uv_client::cached_client::get_cacheable_with_retry
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
 uv_resolver::resolver::process_request request=Prefetch requests *
 uv_client::cached_client::from_path_sync path="/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv"
            0.008127s   0ms TRACE uv_client::cached_client No cache entry exists for /my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
          0.008145s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            0.008175s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.008215s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            0.008224s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            0.008255s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.008263s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.008330s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            0.008353s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            0.008378s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            0.011017s   2ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            0.011577s   3ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            0.014631s   6ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            0.014646s   6ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            0.014711s   6ms TRACE hyper_util::client::legacy::client connection is ready
            0.014725s   6ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            0.042214s  34ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        0.043571s  35ms TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        0.043659s  35ms TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        0.043679s  35ms TRACE uv_client::base_client Cannot retry nested reqwest error
        0.043697s  35ms TRACE uv_client::base_client Retrying error: `unexpected end of file`
        0.043728s  35ms DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
 uv_client::cached_client::from_path_sync path="/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv"
            0.375673s   0ms TRACE uv_client::cached_client No cache entry exists for /my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
          0.375745s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            0.375796s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.375841s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            0.375858s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            0.375882s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.375904s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            0.375974s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            0.375998s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            0.376032s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            0.377285s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            0.377725s   1ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            0.381507s   5ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            0.381555s   5ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            0.381622s   5ms TRACE hyper_util::client::legacy::client connection is ready
            0.381642s   5ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            0.411726s  35ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        0.412491s 404ms TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        0.412539s 404ms TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        0.412557s 404ms TRACE uv_client::base_client Cannot retry nested reqwest error
        0.412572s 404ms TRACE uv_client::base_client Retrying error: `unexpected end of file`
        0.412592s 404ms DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
 uv_client::cached_client::from_path_sync path="/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv"
            2.278101s   0ms TRACE uv_client::cached_client No cache entry exists for /my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
          2.278170s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            2.278204s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.278240s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            2.278256s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            2.278272s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.278288s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            2.278349s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            2.278370s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            2.278395s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            2.279828s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            2.280607s   2ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            2.283763s   5ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            2.283810s   5ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            2.283865s   5ms TRACE hyper_util::client::legacy::client connection is ready
            2.283881s   5ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            2.306153s  27ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        2.307365s   2s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        2.307462s   2s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        2.307476s   2s  TRACE uv_client::base_client Cannot retry nested reqwest error
        2.307487s   2s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
        2.307499s   2s  DEBUG uv_client::cached_client Transient failure while handling response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/; retrying...
       uv_client::cached_client::get_cacheable
         uv_client::cached_client::read_and_parse_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
 uv_client::cached_client::from_path_sync path="/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv"
            6.162349s   0ms TRACE uv_client::cached_client No cache entry exists for /my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
          6.162461s   0ms DEBUG uv_client::cached_client No cache entry for: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
         uv_client::cached_client::fresh_request url="https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/"
            6.162504s   0ms TRACE uv_client::cached_client Sending fresh GET request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            6.162552s   0ms TRACE uv_auth::middleware Handling request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ with authentication policy auto
            6.162567s   0ms TRACE uv_auth::middleware Request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is unauthenticated, checking cache
            6.162587s   0ms TRACE uv_auth::cache No credentials in cache for URL https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            6.162604s   0ms TRACE uv_auth::middleware Attempting unauthenticated request for https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
            6.162684s   0ms TRACE hyper_util::client::legacy::pool checkout waiting for idle connection: ("https", my_artifactory.com)
            6.162709s   0ms DEBUG reqwest::connect starting new connection: https://my_artifactory.com/
            6.162740s   0ms TRACE hyper_util::client::legacy::connect::http Http::connect; scheme=Some("https"), host=Some("my_artifactory.com"), port=None
            6.164157s   1ms DEBUG hyper_util::client::legacy::connect::http connecting to x.x.x.x:443
            6.165047s   2ms DEBUG hyper_util::client::legacy::connect::http connected to x.x.x.x:443
            6.166834s   4ms TRACE hyper_util::client::legacy::client http1 handshake complete, spawning background dispatcher task
            6.166883s   4ms TRACE hyper_util::client::legacy::client waiting for connection to be ready
            6.166945s   4ms TRACE hyper_util::client::legacy::client connection is ready
            6.166965s   4ms TRACE hyper_util::client::legacy::pool checkout dropped for ("https", my_artifactory.com)
            6.184725s  22ms TRACE uv_client::httpcache Response from https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ is storable because it has a heuristically cacheable status code 200
         uv_client::cached_client::new_cache file=/my_home/.cache/uv/simple-v17/index/5b298b0f3b1364ab/requests.rkyv
         uv_client::registry_client::parse_simple_api package=requests
        6.185854s   6s  TRACE uv_client::base_client Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my_artifactory.com")), port: None, path: "/artifactory/api/pypi/pypi-virtual/simple/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" }) } }))), retries: 0 }
        6.185925s   6s  TRACE uv_client::base_client Cannot retry nested reqwest middleware error
        6.185939s   6s  TRACE uv_client::base_client Cannot retry nested reqwest error
        6.185950s   6s  TRACE uv_client::base_client Retrying error: `unexpected end of file`
    6.186193s DEBUG uv::commands::project::add Reverting changes to `pyproject.toml`
    6.186579s DEBUG uv::commands::project::add Removing `uv.lock`
    6.186708s DEBUG uv_fs Released lock at `/my_workspace/test_uv/test_dir/.venv/.lock`
    6.187618s TRACE uv Error trace: Request failed after 3 retries

 Caused by:
     0: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
     1: error decoding response body
     2: request or response body error
     3: error reading a body from connection
     4: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
error: Request failed after 3 retries
  Caused by: Failed to fetch: `https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

Does the fact that `curl` works reliably on this host indicate that the issue is actually with `uv`? Or is there something here that indicates the issue is still with Artifactory?

---

_Comment by @konstin on 2025-09-17 14:59_

According to my understanding of https://github.com/hyperium/h2/pull/743, this should be supported, though there's another report of this problem: https://github.com/seanmonstar/reqwest/issues/2802

---

_Comment by @seanmonstar on 2025-09-17 15:29_

> http1 handshake complete

Looks like the connection was HTTP/1. Does the response include a content-length, or transfer-encoding? Or is it close-delimited?

---

_Comment by @ajrup on 2025-09-17 17:46_

I don't know how to enable those logs within uv, and over wireshark everything is TLS. However, running with `curl-I` this is what I see:

```
$ curl https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/ -o curl_output.xml -I -v
* processing: https://my_artifactory.com/artifactory/api/pypi/pypi-virtual/simple/requests/
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying x.x.x.x:443...
* Connected to my_artifactory.com (x.x.x.x) port 443
* schannel: disabled automatic use of client certificate
* using HTTP/1.x
> HEAD /artifactory/api/pypi/pypi-virtual/simple/requests/ HTTP/1.1
> Host: my_artifactory.com
> User-Agent: curl/8.2.1
> Accept: */*
>
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* HTTP 1.0, assume close after body
< HTTP/1.0 200
< Date: Wed, 17 Sep 2025 17:41:10 GMT
< Server: Apache
< Content-Type: text/html
< Transfer-Encoding: chunked
< Connection: close
<
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* Closing connection
* schannel: shutting down SSL/TLS connection with my_artifactory.com port 443
```

Which shows `Transfer-Encoding: chunked` in the header

---

_Comment by @konstin on 2025-09-20 11:47_

Following https://github.com/seanmonstar/reqwest/issues/2802#issuecomment-3304412319, can you try `curl --compressed` and see if that works?

---

_Comment by @Saltsmart on 2025-09-27 08:46_

similar issue when `requires-python = ">=3.12"` (i.e. to use an older version 3.12) in `pyproject.toml` and run `uv sync`, get the following output:
```
error: Request failed after 3 retries
Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
Caused by: client error (SendRequest)
Caused by: connection error Caused by: peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof
```

`curl -v https://github.com/astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86\_64-unknown-linux-gnu-install\_only\_stripped.tar.gz` got:
```
* Host github.com:443 was resolved.
* IPv6: (none)
* IPv4: 20.205.243.166
*   Trying 20.205.243.166:443...
* ALPN: curl offers h2,http/1.1
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
*  CAfile: /usr/lib/ssl/cert.pem
*  CApath: /usr/lib/ssl/certs
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.3 (IN), TLS change cipher, Change cipher spec (1):
* TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_128_GCM_SHA256 / x25519 / id-ecPublicKey
* ALPN: server accepted h2
* Server certificate:
*  subject: CN=github.com
*  start date: Feb  5 00:00:00 2025 GMT
*  expire date: Feb  5 23:59:59 2026 GMT
*  subjectAltName: host "github.com" matched cert's "github.com"
*  issuer: C=GB; ST=Greater Manchester; L=Salford; O=Sectigo Limited; CN=Sectigo ECC Domain Validation Secure Server CA
*  SSL certificate verify ok.
*   Certificate level 0: Public key type EC/prime256v1 (256/128 Bits/secBits), signed using ecdsa-with-SHA256
*   Certificate level 1: Public key type EC/prime256v1 (256/128 Bits/secBits), signed using ecdsa-with-SHA384
*   Certificate level 2: Public key type EC/secp384r1 (384/192 Bits/secBits), signed using ecdsa-with-SHA384
* Connected to github.com (20.205.243.166) port 443
* using HTTP/2
* [HTTP/2] [1] OPENED stream for https://github.com/astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
* [HTTP/2] [1] [:method: GET]
* [HTTP/2] [1] [:scheme: https]
* [HTTP/2] [1] [:authority: github.com]
* [HTTP/2] [1] [:path: /astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz]
* [HTTP/2] [1] [user-agent: curl/8.14.1]
* [HTTP/2] [1] [accept: */*]
> GET /astral-sh/python-build-standalone/releases/download/20250918/cpython-3.12.11%2B20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz HTTP/2
> Host: github.com
> User-Agent: curl/8.14.1
> Accept: */*
> 
* Request completely sent off
* TLSv1.3 (OUT), TLS alert, decode error (562):
* OpenSSL SSL_read: OpenSSL/3.5.1: error:0A000126:SSL routines::unexpected eof while reading, errno 0
* Failed receiving HTTP2 data: 56(Failure when receiving data from the peer)
* Connection #0 to host github.com left intact
curl: (56) OpenSSL SSL_read: OpenSSL/3.5.1: error:0A000126:SSL routines::unexpected eof while reading, errno 0
```

No VPN is used.

---

_Comment by @konstin on 2025-09-29 11:07_

@Saltsmart If curl fails too, that indicates it's a problem with your network configuration, e.g. a corporate proxy, or network provider, and not uv.

---
