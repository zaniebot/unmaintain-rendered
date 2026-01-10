```yaml
number: 14367
title: "`--find-links` URL Handling Regression in uv 0.7.17"
type: issue
state: closed
author: c892836a
labels:
  - bug
assignees: []
created_at: 2025-06-30T05:24:09Z
updated_at: 2025-07-10T02:43:28Z
url: https://github.com/astral-sh/uv/issues/14367
synced_at: 2026-01-10T03:32:45Z
```

# `--find-links` URL Handling Regression in uv 0.7.17

---

_Issue opened by @c892836a on 2025-06-30 05:24_

### Summary

### Describe the bug

When using `uv==0.7.17`, installing packages with a `requirements.txt` file that specifies a custom `--find-links` URL fails with a timeout error. This issue does **not** occur in `uv==0.7.16`.

It appears that uv 0.7.17 does not handle the trailing slash in the `--find-links` URL correctly, resulting in failed requests.

### To Reproduce

**requirements.txt**:
```
--find-links https://example.com/fileserver/packages/
custom-package==1.0.4
```

**Command:**
```bash
uv pip install -r /tmp/requirements.txt
```

**Result:**
```
error: Failed to read `--find-links` URL: https://example.com/fileserver/packages
  Caused by: Failed to fetch: `https://example.com/fileserver/packages`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (http://example.com/fileserver/packages)
  Caused by: operation timed out
```

### Expected behavior

Packages should be installed successfully using the custom find-links repository.

### Additional context

- The same command and requirements file work correctly in `uv==0.7.16`.
- The issue appears when the `--find-links` URL ends with a `/`.
- The URL is a private package repository (URL replaced with `https://example.com/fileserver/packages/` for privacy).

### Possible related

- Change in URL normalization or handling between 0.7.16 and 0.7.17.

### Platform

Linux 5.15.0-1079-azure x86_64 GNU/Linux

### Version

uv 0.7.17

### Python version

Python 3.10

---

_Label `bug` added by @c892836a on 2025-06-30 05:24_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-30 10:19_

---

_Comment by @jtfmumm on 2025-06-30 11:24_

Thank you for reporting this. I'm working on a fix now.

---

_Comment by @jtfmumm on 2025-06-30 11:40_

Are you sure you don't encounter this problem in 0.7.16? Based on my testing, I think that it should have started with 0.7.16 (which introduced a change that normalized URLs by removing a trailing slash). If the problem you're seeing did start with 0.7.17, I'm not able to reproduce your scenario (in which case it would be helpful to know more about your private registry and to see the `-vv` logs).

---

_Comment by @c892836a on 2025-07-01 02:52_

Thank you for the clarification. Let me provide more details about my setup and testing results.

## Setup Details
My private registry is set up using nginx as a file server, with `.whl` files stored in directories that are accessed via folder links for installation.

## Detailed Testing Results

After multiple tests, I've discovered the following behavior patterns:

### Test 1: uv 0.7.16 with command line `--find-links` (FAILS)

**Command:**
```bash
uv pip install redis-cache==1.0.3 --find-links=http://10.248.36.248:30024/shared/ -vv
```

**Result:** This fails with the following error:

```
uv pip install redis-cache==1.0.3 --find-links=http://10.248.36.248:30024/shared/ -vv
DEBUG uv 0.7.16
DEBUG Searching for default Python interpreter in search path
TRACE Searching PATH for executables: python, python3
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Found possible Python executable: /usr/local/bin/python
TRACE Cached interpreter info for Python 3.10.18, skipping probing: /usr/local/bin/python
DEBUG Found `cpython-3.10.18-linux-x86_64-gnu` at `/usr/local/bin/python` (first executable in the search path)
Using Python 3.10.18 environment at: /usr/local
TRACE Checking lock for `/usr/local` at `/tmp/uv-6c2078bb75587c01.lock`
DEBUG Acquired lock for `/usr/local`
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("redis-cache"), version: "1.0.4", path: "/usr/local/lib/python3.10/site-packages/redis_cache-1.0.4.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "1.0.3" }]), index: None, conflict: None }
DEBUG At least one requirement is not satisfied: redis-cache==1.0.3
DEBUG Using request timeout of 30s
TRACE No cache entry exists for /root/.cache/uv/flat-index-v2/html/435a38850d34f431.msgpack
DEBUG No cache entry for: http://10.248.36.248:30024/shared
TRACE Sending fresh GET request for http://10.248.36.248:30024/shared
TRACE Handling request for http://10.248.36.248:30024/shared with authentication policy auto
TRACE Request for http://10.248.36.248:30024/shared is unauthenticated, checking cache
TRACE No credentials in cache for URL http://10.248.36.248:30024/shared
TRACE Attempting unauthenticated request for http://10.248.36.248:30024/shared
DEBUG Received a cross-origin redirect. Removing sensitive headers.
DEBUG Received HTTP 301 Moved Permanently. Redirecting to http://10.248.36.248/shared/
TRACE Handling request for http://10.248.36.248/shared/ with authentication policy auto
TRACE Request for http://10.248.36.248/shared/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://10.248.36.248/shared/
TRACE Attempting unauthenticated request for http://10.248.36.248/shared/
TRACE Request for http://10.248.36.248/shared/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm http://10.248.36.248
DEBUG No netrc file found
TRACE Considering retry of response HTTP 404 Not Found for http://10.248.36.248/shared/
TRACE Cannot retry error: not an IO error
DEBUG Released lock at `/tmp/uv-6c2078bb75587c01.lock`
error: Failed to read `--find-links` URL: http://10.248.36.248:30024/shared
  Caused by: Failed to fetch: `http://10.248.36.248:30024/shared`
  Caused by: HTTP status client error (404 Not Found) for url (http://10.248.36.248/shared/)
```

### Test 2: uv 0.7.16 with requirements.txt (WORKS)

**requirements.txt content:**
```
--find-links http://10.248.36.248:30024/shared/
redis-cache==1.0.3
```

**Command:**
```bash
uv pip install -r requirements.txt -vv
```

**Result:** This works successfully in uv 0.7.16.

### Test 3: uv 0.7.17 with requirements.txt (FAILS)

Using the same requirements.txt file as Test 2:

**Command:**
```bash
uv pip install -r requirements.txt -vv
```

**Result:** This fails in uv 0.7.17, whereas it worked in 0.7.16.

## Summary

The key observation is that there's a difference in behavior between:
1. Using `--find-links` as a command line argument vs. in requirements.txt
2. How uv 0.7.16 vs 0.7.17 handle the requirements.txt approach

In 0.7.16, the requirements.txt method works while the command line method fails. In 0.7.17, both methods appear to fail.

---

_Comment by @jtfmumm on 2025-07-01 09:52_

Thanks for sharing these details! I have a [draft PR](https://github.com/astral-sh/uv/pull/14387) that I think should fix your issue. I'm not sure if it's the best approach yet, but would it be possible for you to build and test it on your use case?

---

_Comment by @c892836a on 2025-07-01 09:59_

Thank you for working on this issue so quickly! I'd be happy to help test your draft PR.

---

_Comment by @jtfmumm on 2025-07-02 07:31_

Thanks! Let me know if it fixes your issue.

---

_Comment by @c892836a on 2025-07-07 09:32_

Thank you for your efforts and for the recent updates.

I tested the latest version (`uv==0.7.19`) but unfortunately, the problem still persists.

- When I use a `--find-links` URL that points to my nginx-based file server directory, I receive the following error:
```
error: Failed to read `--find-links` URL: http://10.248.36.248:30024/shared
  Caused by: Failed to fetch: `http://10.248.36.248:30024/shared`
  Caused by: HTTP status client error (404 Not Found) for url (http://10.248.36.248/shared/)
```


- If I set the `--find-links` URL to a domain `https://somedomain.com/shared/`:
```
error: Failed to read `--find-links` URL: https://somedomain.com/shared
  Caused by: Failed to fetch: `https://somedomain.com/shared`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://somedomain.com/shared/)
  Caused by: operation timed out
```

---

_Comment by @jtfmumm on 2025-07-07 10:49_

Thanks for the update. [That PR](https://github.com/astral-sh/uv/pull/14387) has not been merged yet. If it's possible for you to build and test with it, that would be very helpful. But otherwise, I'll notify you when it's released.

---

_Closed by @zanieb on 2025-07-09 11:50_

---

_Comment by @c892836a on 2025-07-10 02:43_

Testing with uv version 0.7.20 has confirmed that the issue is resolved. Thank you very much for your assistance.

---
