---
number: 13336
title: "Non-deterministic 403 errors using private pypi server with `uv pip install -r requirements.txt`"
type: issue
state: open
author: crestonbunch
labels:
  - bug
  - external
assignees: []
created_at: 2025-05-07T18:49:10Z
updated_at: 2025-05-16T15:57:41Z
url: https://github.com/astral-sh/uv/issues/13336
synced_at: 2026-01-07T13:12:18-06:00
---

# Non-deterministic 403 errors using private pypi server with `uv pip install -r requirements.txt`

---

_Issue opened by @crestonbunch on 2025-05-07 18:49_

### Summary

Hello, we are witnessing an issue with `uv pip install -r requirements.txt`. We are using an instance of pypicloud backed by S3 (we are aware it's unmaintained, but it still works with `pip`, so it should work with `uv pip` too.)

To reproduce, we can have a very simple requirements.txt file that looks like this:

```
--index-url https://pypi.example.com

package-from-private-registry
```

Set up `uv`:
```bash
% uv --version
uv 0.7.2 (Homebrew 2025-04-30)
% uv venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

(Outputs edited)

1. Now invoke `uv pip install -r requirements.txt`:
```
% uv pip install -r requirements.txt 
Using Python 3.11.9 environment at: /Users/creston/.local/share/virtualenvs/creston-VATBnwns
⠴ package-from-private-registry==1.61.0                                                                                                                                                  
error: Failed to fetch: `https://pypi.example.com/api/package/package-from-private-registry/package-from-private-registry-1.61.0-py3-none-any.whl`
  Caused by: HTTP status client error (403 Forbidden) for url (https://pypi-example.s3.amazonaws.com/pypi/a7ea/package-from-private-registry/package-from-private-registry-1.61.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed&x-amz-security-token=scrubbed&Expires=1746729457)
```

2. Now invoke it again (identical command, don't do anything else)
```
% uv pip install -r requirements.txt 
Using Python 3.11.9 environment at: /Users/creston/.local/share/virtualenvs/creston-VATBnwns
⠴ package-from-private-registry==1.61.0                                                                                                                                                  
error: Failed to fetch: `https://pypi.example.com/api/package/package-from-private-registry/package-from-private-registry-1.61.0-py3-none-any.whl`
  Caused by: HTTP status client error (403 Forbidden) for url (https://pypi-example.s3.amazonaws.com/pypi/a7ea/package-from-private-registry/package-from-private-registry-1.61.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed&x-amz-security-token=scrubbed&Expires=1746729457)
```
Exact same result.

3. But do it again a third time (again, identical command) and it suddenly works!
```
% uv pip install -r requirements.txt
Using Python 3.11.9 environment at: /Users/creston/.local/share/virtualenvs/creston-VATBnwns
Resolved 38 packages in 2.79s
Prepared 29 packages in 575ms
Uninstalled 2 packages in 6ms
Installed 29 packages in 99ms
 + attrs==25.3.0
 + boto3==1.38.10
 + botocore==1.38.10
 + cffi==1.17.1
 + deprecated==1.2.18
 + deprecation==2.1.0
 + importlib-metadata==8.6.1
 + jmespath==1.0.1
 + minject==1.5.0
 + opentelemetry-api==1.32.1
 + prometheus-client==0.21.1
 + pycparser==2.22
 + pyjwt==2.10.1
 + pymemcache==4.0.0
 + pynacl==1.5.0
 + redis==6.0.0
 - requests==2.31.0
 + requests==2.32.3
 + s3transfer==0.12.0
 + sentry-sdk==2.27.0
 + types-protobuf==6.30.2.20250506
 + types-pyyaml==6.0.12.20250402
 + types-requests==2.32.0.20250328
 - typing-extensions==4.12.2
 + typing-extensions==4.13.2
 + wrapt==1.17.2
 + zipp==3.21.0
``` 

This issue started recently (perhaps in the last 1-2 days). Before we never had trouble with this setup. But it's not clear that anything has changed with `uv`. Regardless, it's reproducible on multiple versions of `uv` including the latest 0.7.2.

Sometimes the failure happens 0 or 1 times, but never more than twice. Not sure if that's a clue or just coincidence.

### Platform

macOS arm64 15.3.2 (24D81)

### Version

0.7.2 (and others)

### Python version

3.11.9 (and others)

---

_Label `bug` added by @crestonbunch on 2025-05-07 18:49_

---

_Assigned to @jtfmumm by @konstin on 2025-05-07 18:54_

---

_Renamed from "Non-deterministic failures using private pypi server with `uv pip install -r requirements.txt`" to "Non-deterministic 403 errors using private pypi server with `uv pip install -r requirements.txt`" by @crestonbunch on 2025-05-07 19:28_

---

_Comment by @zanieb on 2025-05-07 20:08_

Sorry you're running into this.

Hm... since you're using `--index-url` and not `--extra-index-url` I guess #12805 is not relevant here. I'm not sure what the non-determinism could be. We'll probably need full, verbose logs (e.g., `-vv`) to help diagnose what's going on.

---

_Comment by @crestonbunch on 2025-05-07 20:32_

Sure, here's the repro steps with `-vv` added (each command was run immediately after the other). I've scrubbed some sensitive information, but each line should be represented.

```
creston@creston-macbook-pro uv-test % uv venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

```
creston@creston-macbook-pro uv-test % uv pip install -r requirements.txt -vv 
DEBUG uv 0.7.2 (Homebrew 2025-04-30)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.13.2, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.13.2-macos-aarch64-none` at `/Users/creston/scratch/creston/uv-test/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.2 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: package-from-private-registry
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13.2
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: package-from-private-registry*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: package-from-private-registry. remaining choices: 
TRACE Fetching metadata for package-from-private-registry from https://pypi.example.com/simple/package-from-private-registry/
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ is storable because its response has a heuristically cacheable status code 200
TRACE Could not determine freshness lifetime, assuming none exists
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/package-from-private-registry/ does not have a fresh cache because its age is 880 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/package-from-private-registry/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/package-from-private-registry/
TRACE Handling request for https://pypi.example.com/simple/package-from-private-registry/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/package-from-private-registry/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/package-from-private-registry/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/package-from-private-registry/
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.example.com/simple/package-from-private-registry/
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: package-from-private-registry
DEBUG Searching for a compatible version of package-from-private-registry (*)
TRACE Selecting candidate for package-from-private-registry with range * with 28 remote versions
TRACE Selecting candidate for package-from-private-registry with range * with 28 remote versions
TRACE Found candidate for package package-from-private-registry with range * after 1 steps: 0.13.0 version
TRACE Returning candidate for package package-from-private-registry with range * after 1 steps
TRACE Found candidate for package package-from-private-registry with range * after 1 steps: 0.13.0 version
TRACE Returning candidate for package package-from-private-registry with range * after 1 steps
DEBUG Selecting: package-from-private-registry==0.13.0 [compatible] (package_from_private_registry-0.13.0-py3-none-any.whl)
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is storable because its response has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl does not have a fresh cache because its age is 880 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Handling request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://pypi.example.com/simple
DEBUG No netrc file found
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.example.com")), port: None, path: "/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://pypi-example-repo.s3.amazonaws.com/pypi/6e60/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed&x-amz-security-token=scrubbed&Expires=1746736049" }))) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for package_from_private_registry-0.13.0-py3-none-any.whl; streaming wheel
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is storable because its response has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl does not have a fresh cache because its age is 880 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Handling request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://pypi.example.com/simple
TRACE Skipping fetch of credentials for https://pypi.example.com/simple, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.example.com")), port: None, path: "/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://pypi-example-repo.s3.amazonaws.com/pypi/6e60/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed&x-amz-security-token=scrubbed&Expires=1746736049" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Released lock at `/Users/creston/scratch/creston/uv-test/.venv/.lock`
error: Failed to fetch: `https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl`
  Caused by: HTTP status client error (403 Forbidden) for url (https://pypi-example-repo.s3.amazonaws.com/pypi/6e60/package-from-private-registry/package_from_private-registry-0.13.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed&x-amz-security-token=scrubbed&Expires=1746736049)
```

```
creston@creston-macbook-pro uv-test % uv pip install -r requirements.txt -vv
DEBUG uv 0.7.2 (Homebrew 2025-04-30)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.13.2, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.13.2-macos-aarch64-none` at `/Users/creston/scratch/creston/uv-test/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.13.2 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: package-from-private-registry
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.13.2
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: package-from-private-registry*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: package-from-private-registry. remaining choices: 
TRACE Fetching metadata for package-from-private-registry from https://pypi.example.com/simple/package-from-private-registry/
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ is storable because its response has a heuristically cacheable status code 200
TRACE Could not determine freshness lifetime, assuming none exists
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/package-from-private-registry/ does not have a fresh cache because its age is 2 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/package-from-private-registry/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/package-from-private-registry/
TRACE Handling request for https://pypi.example.com/simple/package-from-private-registry/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/package-from-private-registry/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/package-from-private-registry/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/package-from-private-registry/
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.example.com/simple/package-from-private-registry/
TRACE Cached request https://pypi.example.com/simple/package-from-private-registry/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: package-from-private-registry
DEBUG Searching for a compatible version of package-from-private-registry (*)
TRACE Selecting candidate for package-from-private-registry with range * with 28 remote versions
TRACE Selecting candidate for package-from-private-registry with range * with 28 remote versions
TRACE Found candidate for package package-from-private-registry with range * after 1 steps: 0.13.0 version
TRACE Returning candidate for package package-from-private-registry with range * after 1 steps
TRACE Found candidate for package package-from-private-registry with range * after 1 steps: 0.13.0 version
DEBUG Selecting: package-from-private-registry==0.13.0 [compatible] (package_from_private_registry-0.13.0-py3-none-any.whl)
TRACE Returning candidate for package package-from-private-registry with range * after 1 steps
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is storable because its response has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl does not have a fresh cache because its age is 882 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Handling request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://pypi.example.com/simple
DEBUG No netrc file found
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.example.com")), port: None, path: "/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://pypi-example-repo.s3.amazonaws.com/pypi/6e60/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl?AWSAccessKeyId=scrubbed&Signature=scrubbed%3D&x-amz-security-token=scrubbed&Expires=1746736051" }))) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for package_from_private_registry-0.13.0-py3-none-any.whl; streaming wheel
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is storable because its response has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Cached request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl does not have a fresh cache because its age is 882 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Handling request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Resource is not modified because old and new etag values ([34, 52, 54, 97, 100, 99, 51, 97, 97, 48, 50, 57, 49, 49, 50, 50, 99, 100, 53, 102, 56, 97, 54, 54, 98, 56, 99, 102, 53, 53, 50, 51, 98, 34]) match
DEBUG Found not-modified response for: https://pypi.example.com/api/package/package-from-private-registry/package_from_private_registry-0.13.0-py3-none-any.whl
TRACE Received built distribution metadata for: package-from-private-registry==0.13.0
DEBUG Adding transitive dependency for package-from-private-registry==0.13.0: sentry-sdk>=1.44.1
DEBUG Adding transitive dependency for package-from-private-registry==0.13.0: typing-extensions>=4.13.2
TRACE Fetching metadata for sentry-sdk from https://pypi.example.com/simple/sentry-sdk/
TRACE Assigned packages: root==0a0.dev0, package-from-private-registry==0.13.0
TRACE Chose package for decision: sentry-sdk. remaining choices: typing-extensions
TRACE Fetching metadata for typing-extensions from https://pypi.example.com/simple/typing-extensions/
TRACE Cached request https://pypi.example.com/simple/typing-extensions/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pypi.example.com/simple/typing-extensions/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/typing-extensions/ does not have a fresh cache because its age is 883 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/typing-extensions/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/typing-extensions/
TRACE Handling request for https://pypi.example.com/simple/typing-extensions/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/typing-extensions/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/typing-extensions/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/typing-extensions/
TRACE Cached request https://pypi.example.com/simple/sentry-sdk/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pypi.example.com/simple/sentry-sdk/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/sentry-sdk/ does not have a fresh cache because its age is 883 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/sentry-sdk/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/sentry-sdk/
TRACE Handling request for https://pypi.example.com/simple/sentry-sdk/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/sentry-sdk/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/sentry-sdk/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/sentry-sdk/
TRACE Resource is not modified because old and new etag values ([34, 76, 65, 106, 69, 72, 53, 77, 52, 53, 54, 49, 67, 43, 110, 55, 48, 111, 78, 116, 111, 71, 81, 34]) match
DEBUG Found not-modified response for: https://pypi.example.com/simple/sentry-sdk/
TRACE Resource is not modified because old and new etag values ([34, 52, 81, 70, 43, 48, 76, 119, 121, 78, 71, 104, 78, 86, 89, 65, 100, 68, 71, 110, 99, 119, 103, 34]) match
DEBUG Found not-modified response for: https://pypi.example.com/simple/typing-extensions/
TRACE Received package metadata for: sentry-sdk
DEBUG Searching for a compatible version of sentry-sdk (>=1.44.1)
TRACE Selecting candidate for sentry-sdk with range >=1.44.1 with 273 remote versions
TRACE Found candidate for package sentry-sdk with range >=1.44.1 after 1 steps: 2.27.0 version
TRACE Returning candidate for package sentry-sdk with range >=1.44.1 after 1 steps
DEBUG Selecting: sentry-sdk==2.27.0 [compatible] (sentry_sdk-2.27.0-py2.py3-none-any.whl)
TRACE Received package metadata for: typing-extensions
TRACE Selecting candidate for sentry-sdk with range >=1.44.1 with 273 remote versions
TRACE Found candidate for package sentry-sdk with range >=1.44.1 after 1 steps: 2.27.0 version
TRACE Returning candidate for package sentry-sdk with range >=1.44.1 after 1 steps
TRACE Selecting candidate for typing-extensions with range >=4.13.2 with 44 remote versions
TRACE Found candidate for package typing-extensions with range >=4.13.2 after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range >=4.13.2 after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/dd/8b/fb496a45854e37930b57564a20fb8e90dd0f8b6add0491527c00f2163b00/sentry_sdk-2.27.0-py2.py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dd/8b/fb496a45854e37930b57564a20fb8e90dd0f8b6add0491527c00f2163b00/sentry_sdk-2.27.0-py2.py3-none-any.whl.metadata
TRACE Received built distribution metadata for: sentry-sdk==2.27.0
TRACE Cached request https://files.pythonhosted.org/packages/8b/54/b1ae86c0973cc6f0210b53d508ca3641fb6d0c56823f288d108bc7ab3cc8/typing_extensions-4.13.2-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8b/54/b1ae86c0973cc6f0210b53d508ca3641fb6d0c56823f288d108bc7ab3cc8/typing_extensions-4.13.2-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: typing-extensions==4.13.2
DEBUG Adding transitive dependency for sentry-sdk==2.27.0: urllib3>=1.26.11
DEBUG Adding transitive dependency for sentry-sdk==2.27.0: certifi*
TRACE Fetching metadata for urllib3 from https://pypi.example.com/simple/urllib3/
TRACE Assigned packages: root==0a0.dev0, package-from-private-registry==0.13.0, sentry-sdk==2.27.0
TRACE Chose package for decision: typing-extensions. remaining choices: certifi, urllib3
DEBUG Searching for a compatible version of typing-extensions (>=4.13.2)
TRACE Selecting candidate for typing-extensions with range >=4.13.2 with 44 remote versions
TRACE Fetching metadata for certifi from https://pypi.example.com/simple/certifi/
TRACE Found candidate for package typing-extensions with range >=4.13.2 after 1 steps: 4.13.2 version
TRACE Returning candidate for package typing-extensions with range >=4.13.2 after 1 steps
DEBUG Selecting: typing-extensions==4.13.2 [compatible] (typing_extensions-4.13.2-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, package-from-private-registry==0.13.0, sentry-sdk==2.27.0, typing-extensions==4.13.2
TRACE Chose package for decision: urllib3. remaining choices: certifi
TRACE Cached request https://pypi.example.com/simple/certifi/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pypi.example.com/simple/certifi/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/certifi/ does not have a fresh cache because its age is 882 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/certifi/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/certifi/
TRACE Handling request for https://pypi.example.com/simple/certifi/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/certifi/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/certifi/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/certifi/
TRACE Cached request https://pypi.example.com/simple/urllib3/ is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
TRACE Cached request https://pypi.example.com/simple/urllib3/ has a cached response that does not allow staleness
TRACE Request https://pypi.example.com/simple/urllib3/ does not have a fresh cache because its age is 883 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.example.com/simple/urllib3/
DEBUG Sending revalidation request for: https://pypi.example.com/simple/urllib3/
TRACE Handling request for https://pypi.example.com/simple/urllib3/ with authentication policy auto
TRACE Request for https://pypi.example.com/simple/urllib3/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.example.com/simple/urllib3/
TRACE Attempting unauthenticated request for https://pypi.example.com/simple/urllib3/
TRACE Resource is not modified because old and new etag values ([34, 116, 122, 121, 49, 113, 43, 114, 76, 68, 103, 85, 102, 119, 47, 70, 117, 122, 100, 56, 79, 103, 81, 34]) match
DEBUG Found not-modified response for: https://pypi.example.com/simple/urllib3/
TRACE Resource is not modified because old and new etag values ([34, 85, 70, 73, 69, 77, 73, 116, 49, 72, 50, 116, 99, 48, 104, 47, 103, 90, 72, 84, 101, 57, 65, 34]) match
DEBUG Found not-modified response for: https://pypi.example.com/simple/certifi/
TRACE Received package metadata for: urllib3
DEBUG Searching for a compatible version of urllib3 (>=1.26.11)
TRACE Selecting candidate for urllib3 with range >=1.26.11 with 98 remote versions
TRACE Found candidate for package urllib3 with range >=1.26.11 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range >=1.26.11 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
TRACE Received package metadata for: certifi
TRACE Selecting candidate for urllib3 with range >=1.26.11 with 98 remote versions
TRACE Found candidate for package urllib3 with range >=1.26.11 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range >=1.26.11 after 1 steps
TRACE Selecting candidate for certifi with range * with 63 remote versions
TRACE Found candidate for package certifi with range * after 1 steps: 2025.4.26 version
TRACE Returning candidate for package certifi with range * after 1 steps
TRACE Cached request https://files.pythonhosted.org/packages/6b/11/cc635220681e93a0183390e26485430ca2c7b5f9d33b15c74c2861cb8091/urllib3-2.4.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/11/cc635220681e93a0183390e26485430ca2c7b5f9d33b15c74c2861cb8091/urllib3-2.4.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: urllib3==2.4.0
TRACE Cached request https://files.pythonhosted.org/packages/4a/7e/3db2bd1b1f9e95f7cddca6d6e75e2f2bd9f51b1246e546d88addca0106bd/certifi-2025.4.26-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4a/7e/3db2bd1b1f9e95f7cddca6d6e75e2f2bd9f51b1246e546d88addca0106bd/certifi-2025.4.26-py3-none-any.whl.metadata
TRACE Assigned packages: root==0a0.dev0, package-from-private-registry==0.13.0, sentry-sdk==2.27.0, typing-extensions==4.13.2, urllib3==2.4.0
TRACE Chose package for decision: certifi. remaining choices: 
DEBUG Searching for a compatible version of certifi (*)
TRACE Selecting candidate for certifi with range * with 63 remote versions
TRACE Received built distribution metadata for: certifi==2025.4.26
TRACE Found candidate for package certifi with range * after 1 steps: 2025.4.26 version
TRACE Returning candidate for package certifi with range * after 1 steps
DEBUG Selecting: certifi==2025.4.26 [compatible] (certifi-2025.4.26-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, package-from-private-registry==0.13.0, sentry-sdk==2.27.0, typing-extensions==4.13.2, urllib3==2.4.0, certifi==2025.4.26
DEBUG Tried 5 versions: certifi 1, package-from-private-registry 1, sentry-sdk 1, typing-extensions 1, urllib3 1
DEBUG marker environment resolution took 0.842s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.2", version: "3.13.2" }, os_name: "posix", platform_machine: "arm64", platform_python_implementation: "CPython", platform_release: "24.3.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.3.0: Thu Jan  2 20:24:23 PST 2025; root:xnu-11215.81.4~3/RELEASE_ARM64_T6020", python_full_version: StringVersion { string: "3.13.2", version: "3.13.2" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> package-from-private-registry
TRACE Resolution edge:     0a0.dev0 -> 0.13.0
TRACE Resolution edge: package-from-private-registry -> sentry-sdk
TRACE Resolution edge:     0.13.0 -> 2.27.0
TRACE Resolution edge: package-from-private-registry -> typing-extensions
TRACE Resolution edge:     0.13.0 -> 4.13.2
TRACE Resolution edge: sentry-sdk -> urllib3
TRACE Resolution edge:     2.27.0 -> 2.4.0
TRACE Resolution edge: sentry-sdk -> certifi
TRACE Resolution edge:     2.27.0 -> 2025.4.26
Resolved 5 packages in 843ms
DEBUG Registry requirement already cached: package-from-private-registry==0.13.0
DEBUG Registry requirement already cached: certifi==2025.4.26
DEBUG Registry requirement already cached: typing-extensions==4.13.2
DEBUG Registry requirement already cached: urllib3==2.4.0
DEBUG Registry requirement already cached: sentry-sdk==2.27.0
TRACE Extracting file name=PackageName("urllib3")
TRACE Extracting file name=PackageName("typing-extensions")
TRACE Extracting file name=PackageName("certifi")
TRACE Extracting file name=PackageName("sentry-sdk")
TRACE Extracting file name=PackageName("package-from-private-registry")
TRACE Cloning /Users/creston/.cache/uv/archive-v0/UCptkxz3a8loqgC-QbWrI/urllib3-2.4.0.dist-info to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/urllib3-2.4.0.dist-info
TRACE Cloning /Users/creston/.cache/uv/archive-v0/5iz4h4k4TO343CsvaPMA3/certifi-2025.4.26.dist-info to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/certifi-2025.4.26.dist-info
TRACE Cloning /Users/creston/.cache/uv/archive-v0/o82tXBMdWCFLh4YZAz8iO/sentry_sdk to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/sentry_sdk
TRACE Cloning /Users/creston/.cache/uv/archive-v0/Bp_DlyLG4dYkBOTH3w8LH/package_from_private_registry to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/package_from_private_registry
TRACE Cloning /Users/creston/.cache/uv/archive-v0/HWGeT7O2hy6RRbyo72h82/typing_extensions-4.13.2.dist-info to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/typing_extensions-4.13.2.dist-info
TRACE Cloning /Users/creston/.cache/uv/archive-v0/5iz4h4k4TO343CsvaPMA3/certifi to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/certifi
TRACE Cloning /Users/creston/.cache/uv/archive-v0/UCptkxz3a8loqgC-QbWrI/urllib3 to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/urllib3
TRACE Cloning /Users/creston/.cache/uv/archive-v0/HWGeT7O2hy6RRbyo72h82/typing_extensions.py to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/typing_extensions.py
TRACE Extracted 2 files name=PackageName("typing-extensions")
TRACE Cloning /Users/creston/.cache/uv/archive-v0/Bp_DlyLG4dYkBOTH3w8LH/package_from_private_registry-0.13.0.dist-info to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/package_from_private_registry-0.13.0.dist-info
TRACE No entrypoints name=PackageName("typing-extensions")
TRACE No data name=PackageName("typing-extensions")
TRACE Writing installer metadata name=PackageName("typing-extensions")
TRACE Extracted 2 files name=PackageName("certifi")
TRACE No entrypoints name=PackageName("certifi")
TRACE No data name=PackageName("certifi")
TRACE Writing installer metadata name=PackageName("certifi")
TRACE Extracted 2 files name=PackageName("urllib3")
TRACE No entrypoints name=PackageName("urllib3")
TRACE No data name=PackageName("urllib3")
TRACE Writing installer metadata name=PackageName("urllib3")
TRACE Cloning /Users/creston/.cache/uv/archive-v0/o82tXBMdWCFLh4YZAz8iO/sentry_sdk-2.27.0.dist-info to /Users/creston/scratch/creston/uv-test/.venv/lib/python3.13/site-packages/sentry_sdk-2.27.0.dist-info
TRACE Extracted 2 files name=PackageName("package-from-private-registry")
TRACE Extracted 2 files name=PackageName("sentry-sdk")
TRACE No entrypoints name=PackageName("package-from-private-registry")
TRACE No data name=PackageName("package-from-private-registry")
TRACE Writing installer metadata name=PackageName("package-from-private-registry")
TRACE Writing record name=PackageName("urllib3")
TRACE Writing record name=PackageName("typing-extensions")
TRACE Writing record name=PackageName("certifi")
TRACE No entrypoints name=PackageName("sentry-sdk")
TRACE No data name=PackageName("sentry-sdk")
TRACE Writing installer metadata name=PackageName("sentry-sdk")
TRACE Writing record name=PackageName("package-from-private-registry")
TRACE Writing record name=PackageName("sentry-sdk")
Installed 5 packages in 10ms
 + certifi==2025.4.26
 + package-from-private-registry==0.13.0
 + sentry-sdk==2.27.0
 + typing-extensions==4.13.2
 + urllib3==2.4.0
DEBUG Released lock at `/Users/creston/scratch/creston/uv-test/.venv/.lock`
```

---

_Referenced in [astral-sh/uv#13338](../../astral-sh/uv/issues/13338.md) on 2025-05-07 21:28_

---

_Comment by @zanieb on 2025-05-07 21:34_

How are those tokens getting attached to the URLs? Are they in the links from your index's API response? If so, is it feasible that the tokens are just expiring during the failing case and are refreshed on succeeding case?

---

_Comment by @zanieb on 2025-05-07 21:42_

Digging in the history... found a related issue

- https://github.com/astral-sh/uv/issues/2025

---

_Comment by @crestonbunch on 2025-05-07 21:45_

- pypicloud generates a URL for each package (`example.com/my-package/my_package-0.13.0-py3-none-any.whl`
- This is a `302 Found` redirect to a [pre-signed S3 URL](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-presigned-url.html). Anyone with the URL can download the object even if there is no public ACL
- I can confirm that clicking the pre-signed S3 URL and opening it in my web browser returns a 200 success and downloads the package
- The tokens expire after 24 hours, which is longer than the tests. We have seen issues where things that cache full S3 url (and not the redirect URL) will fail after 24 hours, but that's not the case here since we're dealing with time scales on the order of seconds.
- I have been playing around with uv's caching flags to try and get it to consistently fail, but even the most aggressive flags (`--reinstall --refresh --no-cache`) will still succeed after a few attempts, which makes me think it's either (1) not a caching issue or (2) `--no-cache` is a lie


---

_Comment by @zanieb on 2025-05-07 21:52_

Thanks for clarifying! Alas, I don't have a theory right now.

Regarding (2), `--no-cache` will still cache _within_ process, but it won't share a cache with other processes.



---

_Comment by @crestonbunch on 2025-05-07 21:52_

I've looked at #2025, but that seems unrelated. It is true that you cannot make a `HEAD` request for pre-signed S3 URLs. The linked PR fixes that issue by making the HEAD request optional. My (uninformed) tour through the [code](https://github.com/astral-sh/uv/blob/3c413f74b91118548779b06a01db61be27f560ca/crates/uv-client/src/registry_client.rs#L873) makes me think that is still the case, but perhaps there was a regression somewhere else. I haven't found a `uv` version that works, but I also haven't looked particularly hard.

Update: I went all the way back to `uv==0.2.0` and every minor version had the same error:

```
creston@creston-macbook-pro uv-test % uv clean && uv venv && uv pip install uv==0.2.0 && uv run uv --version && uv run uv pip install -r requirements.txt
Clearing cache at: /Users/creston/.cache/uv
Removed 24 files (22.8MiB)
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 1 package in 205ms
Prepared 1 package in 789ms
Installed 1 package in 1ms
 + uv==0.2.0
uv 0.2.0 (0a87391d5 2024-05-22)
⠼ my-package==0.13.0                                                                                                                                                    error: Failed to download `my-package==0.13.0`
  Caused by: HTTP status client error (403 Forbidden) for url ...
```

---

_Comment by @crestonbunch on 2025-05-08 01:39_

I believe it is a bug (feature?) of `reqwest`. It appears to "cache" a `HEAD` error response for a subsequent `GET` request to the same URL. It's reasonable to assume the response would be the same. The nondeterminism is because it only happens when it reuses a connection. I haven't looked into the reqwest source to confirm, but I can reproduce the issue with the following code:

<details><summary>Cargo.toml</summary>
<p>

```toml
[package]
name = "reqwest-test"
version = "0.1.0"
edition = "2024"

[dependencies]
reqwest = { version = "0.12.7", features = ["json"] }
tokio = { version = "1", features = ["full"] }
```

</p>
</details> 

```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    // whatever package URL, you can also use any pre-signed S3 URL here, the behavior will be
    // reproducible (i.e., the 302 redirect is not relevant)
    let url = "https://pypi.example.com/api/package/my-package/my_package-0.13.0-py3-none-any.whl";
    println!("Downloading: {}", url);
    
    let client = reqwest::ClientBuilder::new()
        .pool_max_idle_per_host(20) // same pool size as uv
        .build()?;
    let head_response = client.head(url).send().await;
    println!("HEAD response: {:?}", head_response); // returns 403 for pre-signed S3 urls
    let response = client.get(url).send().await?;
    println!("GET response: {:?}", &response); // sometimes returns 403 because it's caching the HEAD request(?)
    response.bytes().await?;
    Ok(())
}
```

This code is also non-deterministic. Multiple runs will sometimes return 403 and sometimes 200. But if you remove the `HEAD` request, it's always a 200 response.

I suspect this is probably desirable behavior in 99.9% of cases that use reqwest, and it's a bad idea to depend on HEAD and GET returning different status codes in the way that `uv` does. However, it's also clearly a niche situation that only applies to URLs like S3 pre-signed URLs. Arguably S3 is doing the wrong thing by rejecting a HEAD request.

In the uv source, the corresponding requests are:
- [HEAD](https://github.com/astral-sh/uv/blob/145fe4e7e3a941a7396703a00c357b1455204c12/crates/uv-client/src/registry_client.rs#L915-L924)
- [GET](https://github.com/astral-sh/uv/blob/145fe4e7e3a941a7396703a00c357b1455204c12/crates/uv-client/src/registry_client.rs#L974-L977)

EDIT:
- Setting `.pool_max_idle_per_host(0)` reliably fixes the issue. The connection can never be reused, so the GET request never returns 403. This is not configurable in `uv`. One workaround would be to make this configurable, but that feels like a bad long-term solution.
- An alternative, more elegant solution, would be to simply add `.header(reqwest::header::CONNECTION, "close")` to the HEAD request so that it is forced to close the connection. (The response will not be re-used.) This also reliably solves the problem, even with `pool_max_idle_per_host(20)`.
- A more robust solution, but I'm not sure how it would be implemented, is to have it only close the connection when it gets an error status, instead of re-using it.

---

_Comment by @zanieb on 2025-05-08 02:03_

Wow, thanks for digging! I really appreciate it.

---

_Assigned to @zanieb by @zanieb on 2025-05-08 02:05_

---

_Comment by @zanieb on 2025-05-08 02:09_

cc @seanmonstar (might be interesting to you)

---

_Comment by @seanmonstar on 2025-05-09 14:44_

Just poking in briefly: reqwest doesn't do any caching of responses at all. What you see is what the server sent.

A search suggests that S3 pre-signed URLs _can_ support HEAD requests, but that the signature includes the request method, so you need a different signature for HEAD vs GET. 🤷 

---

_Comment by @zanieb on 2025-05-09 15:10_

> Just poking in briefly: reqwest doesn't do any caching of responses at all. What you see is what the server sent.

Thanks!

So, it sounds like, server-side, there's non-deterministic return behavior when a connection is re-used.

---

_Comment by @crestonbunch on 2025-05-09 21:35_

Thanks, that explanation makes a lot more sense. I further tested with a simple Flask server and confirmed that all GET requests return 200. That supports the theory that this is a server-side issue with S3.

I'm also having difficulty reproducing this issue, even with S3 URLs. AWS may have introduced this bug and quietly reverted it sometime in the last 24 hours. This would explain the mystery around the sudden occurrence of this bug. We used `uv` with a pypicloud instance for weeks before hitting these errors. Suddenly, on 2025-05-07, the errors started happening. Now they've stopped without us changing a single thing.

---

_Comment by @zanieb on 2025-05-09 21:40_

Wow.. haha

Well, if that's the case, I'm glad it's working again!

---

_Label `external` added by @zanieb on 2025-05-09 21:41_

---

_Comment by @crestonbunch on 2025-05-16 15:57_

I'm happy to close this issue, unless you think there's something uv should do differently. We've been error-free for a week now, and it's pretty clear at this point that the problem wasn't inherent to uv.

---
