```yaml
number: 16073
title: Project is published even when upload check fails due to Unautherized 401
type: issue
state: open
author: brainslush
labels:
  - bug
assignees: []
created_at: 2025-09-30T11:20:36Z
updated_at: 2026-01-12T20:10:34Z
url: https://github.com/astral-sh/uv/issues/16073
synced_at: 2026-01-12T20:26:32Z
```

# Project is published even when upload check fails due to Unautherized 401

---

_@brainslush_

### Summary

If you provide the wrong or forget to set the index credentials (not the publishing credentials), publishing to a secured private index with
`uv publish --index privateindex`
succeeds with a warning.

pyproject.toml
```toml
#...
[[tool.uv.index]]
name = "privateindex"
url = "https://privateindex.org/simple/"
publish-url = "https://privateindex.org/legacy/"
explicit = true
#...
```

Log
```
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
WARN Package not found in the registry; skipping upload check for ....
Uploading ....
```

I would expect this process to fail for any response code other than 404. 
Is this by design?
I would suggest either to fix the behaviour or add an option to the cli which enforces that the check must succeed.

I would be willing to contribute a fix if interested.

### Platform

Ubuntu 24.04

### Version

0.8.22

### Python version

Python 3.12.11

---

_Label `bug` added by @brainslush on 2025-09-30 11:20_

---

_Assigned to @konstin by @zanieb on 2025-09-30 14:47_

---

_Comment by @hutch3232 on 2026-01-12 19:24_

I came looking to post approximately this same issue. Quick summary of my issue:

I use a private package repo on Artifactory. I'm using CICD to `build` and `publish` with `uv`. I found that if I triggered the publish process without incrementing the version, I was overwriting previously released versions (and breaking hashes). The `--check-url` stuff worked locally, it was only breaking in CICD. Found it was because in my local environment I had a `.netrc` with creds for Artifactory, but not in CICD. The curious thing is that it is able to use command line passed creds to publish, but it couldn't use those same creds for the check?

`pyproject.toml`
```
[[tool.uv.index]]
name = "artifactory"
url = "https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple"
publish-url = "https://artifactory.myco.com/artifactory/api/pypi/pypi-local"
```

```
+ python3.9 -m uv build
Building source distribution (uv build backend)...
Building wheel from source distribution (uv build backend)...
Successfully built dist/mypkg-0.0.31.tar.gz
Successfully built dist/mypkg-0.0.31-py3-none-any.whl

+ python3.9 -m uv publish --verbose --verbose --dry-run --index artifactory -u hutch3232 -p ****
DEBUG uv 0.9.7
DEBUG Publishing with index artifactory
DEBUG Not a distribution filename: `.gitignore`
Checking 2 files against https://artifactory.myco.com/artifactory/api/pypi/pypi-local
DEBUG Using request timeout of 900s
DEBUG Checking for mypkg-0.0.31-py3-none-any.whl in the registry
TRACE Fetching metadata for mypkg from https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Response from https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ is storable because it has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Request to https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ does not have a fresh cache entry because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
DEBUG Sending revalidation request for: https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Handling request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ with authentication policy auto
TRACE Request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Attempting unauthenticated request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://artifactory.myco.com/artifactory/api/pypi/pypi/simple
TRACE No credentials in cache for realm https://artifactory.myco.com/
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `/export/home/cicdjenk/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Acquired lock for `credentials store`
DEBUG Released lock at `/export/home/cicdjenk/.local/share/uv/credentials/credentials.toml.lock`
DEBUG No credentials file found at /export/home/cicdjenk/.local/share/uv/credentials/credentials.toml
TRACE Considering retry of response HTTP 403 Forbidden for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Cannot retry nested reqwest middleware error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
WARN Package not found in the registry; skipping upload check for mypkg-0.0.31-py3-none-any.whl
Checking mypkg-0.0.31-py3-none-any.whl (63.0KiB)
DEBUG Hashing dist/mypkg-0.0.31-py3-none-any.whl
DEBUG Skipping validation request for unsupported publish URL: https://artifactory.myco.com/artifactory/api/pypi/pypi-local
DEBUG Checking for mypkg-0.0.31.tar.gz in the registry
TRACE Fetching metadata for mypkg from https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Response from https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ is storable because it has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Request to https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ does not have a fresh cache entry because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
DEBUG Sending revalidation request for: https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Handling request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ with authentication policy auto
TRACE Request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Attempting unauthenticated request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Request for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://artifactory.myco.com/artifactory/api/pypi/pypi/simple
TRACE No credentials in cache for realm https://artifactory.myco.com/
TRACE Skipping fetch of credentials for https://artifactory.myco.com/artifactory/api/pypi/pypi/simple, previous attempt failed
TRACE Considering retry of response HTTP 403 Forbidden for https://artifactory.myco.com/artifactory/api/pypi/pypi-local/simple/mypkg/
TRACE Cannot retry nested reqwest middleware error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
WARN Package not found in the registry; skipping upload check for mypkg-0.0.31.tar.gz
Checking mypkg-0.0.31.tar.gz (49.5KiB)
DEBUG Hashing dist/mypkg-0.0.31.tar.gz
DEBUG Skipping validation request for unsupported publish URL: https://artifactory.myco.com/artifactory/api/pypi/pypi-local
```

The log is running with `--dry-run` so it isn't actually publishing, but we can see it disabled the check. The funny thing is that if I took out `--dry-run` it actually succeeds to upload. I would think if it can upload, the check should be able to succeed.

Thank you for the incredible package!

---

_Comment by @zanieb on 2026-01-12 20:06_

Some indexes use different credentials for write (publish) and read (check)

---

_Comment by @hutch3232 on 2026-01-12 20:10_

Thanks! I was able to resolve nicely by setting `UV_INDEX_ARTIFACTORY_USERNAME` and `UV_INDEX_ARTIFACTORY_PASSWORD`

---
