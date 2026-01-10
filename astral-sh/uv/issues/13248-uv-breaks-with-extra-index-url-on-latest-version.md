---
number: 13248
title: uv breaks with --extra-index-url on latest version
type: issue
state: open
author: desmondcheongzx
labels:
  - bug
assignees: []
created_at: 2025-05-01T06:51:37Z
updated_at: 2025-05-05T08:59:56Z
url: https://github.com/astral-sh/uv/issues/13248
synced_at: 2026-01-10T01:25:30Z
---

# uv breaks with --extra-index-url on latest version

---

_Issue opened by @desmondcheongzx on 2025-05-01 06:51_

### Summary

We used to run `uv pip install -r requirements-dev.txt daft --pre --extra-index-url https://d1p3klp2t5517h.cloudfront.net/builds/nightly --force-reinstall` in our nightly CI with no issue. For the past few days this results in
```
  × No solution found when resolving dependencies:
  ╰─▶ Because httpx was not found in the package registry and you require httpx==0.27.2, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://d1p3klp2t5517h.cloudfront.net/builds/nightly) could not be queried due to a lack of valid authentication credentials (403 Forbidden).
```

Possibly because it's trying to resolve `httpx` from our extra index instead of pypi

### Platform

Ubuntu 24.04, Darwin 22.3.0 arm64

### Version

uv 0.7.2

### Python version

_No response_

---

_Label `bug` added by @desmondcheongzx on 2025-05-01 06:51_

---

_Referenced in [Eventual-Inc/Daft#4279](../../Eventual-Inc/Daft/pulls/4279.md) on 2025-05-01 07:06_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-05-01 08:09_

---

_Comment by @jtfmumm on 2025-05-01 08:15_

Thanks for the report! I'm guessing this is related to the 0.7.0 release, but since you say it's been happening for a few days, do you know if this was observed with 0.6.17?

It would be helpful if you could provide the complete trace logs (`-vv`). And what are you using for your extra index? 

---

_Comment by @hwong557 on 2025-05-01 17:39_

Version 0.6.17 works for us, but not 0.7.0. I'm on a jfrog repository.

---

_Comment by @jtfmumm on 2025-05-01 17:49_

> Version 0.6.17 works for us, but not 0.7.0. I'm on a jfrog repository.

Are you also encountering 403s?  Would you mind sharing your trace logs (‘-vv’)?

---

_Comment by @hwong557 on 2025-05-01 17:52_

```
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foobar.jfrog.io")), port: None, path: "/artifactory/api/pypi/pypi/simple/zipp/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/zipp/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: zipp
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioboto3==14.1.0
  aioboto3 not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioboto3==14.1.0
  aioboto3 not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because aioboto3 was not found in the package registry and you
      require aioboto3==14.1.0, we can conclude that your requirements are
      unsatisfiable.

      hint: An index URL
      (https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple) could
      not be queried due to a lack of valid authentication credentials (403
      Forbidden).

```

Here are some partial trace logs.

---

_Comment by @jtfmumm on 2025-05-01 17:59_

@hwong557 Thanks for sharing. Is it possible the credentials are actually invalid? It would be helpful to see the complete trace logs because this partial trace is consistent with correct behavior (failing the index search on authorization failure). Note that on 0.6.17 we treated authorization failure as package not found

---

_Comment by @hwong557 on 2025-05-01 18:07_

We confirmed the credentials are valid. I tested the same credentials on 0.6.17 and it worked.

---

_Comment by @zanieb on 2025-05-01 18:12_

We need the logs in order to help you, i.e., it could have worked because we fetched that package from another package index.

---

_Comment by @zanieb on 2025-05-01 18:13_

If you could reduce this to a single package, e.g., `uv pip install aioboto3 ...` and share verbose logs for both the successful and failing case, with your private information redacted, that'd be great.

---

_Comment by @hwong557 on 2025-05-01 18:32_

Commands
```bash
command -v uv || pip install uv==0.7.0
uv venv
export UV_EXTRA_INDEX_URL=https://${JFROG_USERNAME}:${JFROG_ACCESS_TOKEN}@foobar.jfrog.io/artifactory/api/pypi/pypi/simple
uv pip install -vv aioboto3


```

Trace logs in failing case, run in CICD.


```
Creating virtual environment at: .venv
DEBUG uv 0.7.0
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /codefresh/volume/foobar/.venv/bin/python3
DEBUG Found `cpython-3.12.10-linux-x86_64-gnu` at `/codefresh/volume/foobar/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.10 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: aioboto3
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.10
DEBUG Solving with target Python version: >=3.12.10
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: aioboto3*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: aioboto3. remaining choices:
TRACE Fetching metadata for aioboto3 from https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
 for /root/.cache/uv/simple-v15/index/7166f2fb68f9a269/aioboto3.rkyv
ttps://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
uest for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ with authentication policy auto
***:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foobar.jfrog.io")), port: None, path: "/artifactory/api/pypi/pypi/simple/aioboto3/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: aioboto3
DEBUG Searching for a compatible version of aioboto3 (*)
TRACE Selecting candidate for aioboto3 with range * with 0 remote versions
DEBUG No compatible version found for: aioboto3
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioboto3*
  aioboto3 not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioboto3*
  aioboto3 not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because aioboto3 was not found in the package registry and you require
      aioboto3, we can conclude that your requirements are unsatisfiable.

      hint: An index URL
      (https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple) could
      not be queried due to a lack of valid authentication credentials (403
      Forbidden).
DEBUG Released lock at `/codefresh/volume/foobar/.venv/.lock`


```

The traces in the passing case are much longer and I need to do some fanagling to get it out of our CICD without truncation.

---

_Comment by @zanieb on 2025-05-01 18:43_

Thank you! The second trace will be key here, I think. I tried reproducing with that first trace, and could not. Do you know of any non-default settings you have on JFrog?

---

_Comment by @hwong557 on 2025-05-01 18:50_

Passing trace (I just ran it from local).

```
DEBUG uv 0.6.17
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.12.8, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/home/foobar/work/foobar-service/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.8 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: aioboto3
TRACE Caching credentials for https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple
TRACE Caching credentials for https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.8
DEBUG Solving with target Python version: >=3.12.8
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: aioboto3*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: aioboto3. remaining choices: 
TRACE Fetching metadata for aioboto3 from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioboto3/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: aioboto3
TRACE Selecting candidate for aioboto3 with range * with 73 remote versions
DEBUG Searching for a compatible version of aioboto3 (*)
TRACE Selecting candidate for aioboto3 with range * with 73 remote versions
TRACE Found candidate for package aioboto3 with range * after 1 steps: 14.1.0 version
TRACE Found candidate for package aioboto3 with range * after 1 steps: 14.1.0 version
TRACE Returning candidate for package aioboto3 with range * after 1 steps
TRACE Returning candidate for package aioboto3 with range * after 1 steps
DEBUG Selecting: aioboto3==14.1.0 [compatible] (aioboto3-14.1.0-py3-none-any.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/41/85/04ba3a451a2aad4af64ddb7744620debc43ea9437eb9224186e31f0c984d/aioboto3-14.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/41/85/04ba3a451a2aad4af64ddb7744620debc43ea9437eb9224186e31f0c984d/aioboto3-14.1.0-py3-none-any.whl
TRACE Received built distribution metadata for: aioboto3==14.1.0
DEBUG Adding transitive dependency for aioboto3==14.1.0: aiobotocore[boto3]>=2.21.1, <2.21.1+
DEBUG Adding transitive dependency for aioboto3==14.1.0: aiofiles>=23.2.1
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0
TRACE Fetching metadata for aiobotocore from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
TRACE Chose package for decision: aiobotocore[boto3]. remaining choices: aiofiles
TRACE Fetching metadata for aiofiles from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiofiles/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiobotocore/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: aiofiles
TRACE Selecting candidate for aiofiles with range >=23.2.1 with 14 remote versions
TRACE Found candidate for package aiofiles with range >=23.2.1 after 1 steps: 24.1.0 version
TRACE Returning candidate for package aiofiles with range >=23.2.1 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/a5/45/30bb92d442636f570cb5651bc661f52b610e2eec3f891a5dc3a4c3667db0/aiofiles-24.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/a5/45/30bb92d442636f570cb5651bc661f52b610e2eec3f891a5dc3a4c3667db0/aiofiles-24.1.0-py3-none-any.whl
TRACE Received built distribution metadata for: aiofiles==24.1.0
TRACE Received package metadata for: aiobotocore
DEBUG Searching for a compatible version of aiobotocore[boto3] (>=2.21.1, <2.21.1+)
TRACE Selecting candidate for aiobotocore with range >=2.21.1, <2.21.1+ with 110 remote versions
TRACE Found candidate for package aiobotocore with range >=2.21.1, <2.21.1+ after 1 steps: 2.21.1 version
TRACE Returning candidate for package aiobotocore with range >=2.21.1, <2.21.1+ after 1 steps
DEBUG Selecting: aiobotocore==2.21.1 [compatible] (aiobotocore-2.21.1-py3-none-any.whl)
DEBUG Adding transitive dependency for aiobotocore==2.21.1: aiobotocore==2.21.1
DEBUG Adding transitive dependency for aiobotocore==2.21.1: aiobotocore[boto3]==2.21.1
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0
TRACE Chose package for decision: aiobotocore. remaining choices: aiofiles, aiobotocore[boto3]
DEBUG Searching for a compatible version of aiobotocore (==2.21.1)
TRACE Selecting candidate for aiobotocore with range ==2.21.1 with 110 remote versions
TRACE Selecting candidate for aiobotocore with range ==2.21.1 with 110 remote versions
TRACE Found candidate for package aiobotocore with range ==2.21.1 after 1 steps: 2.21.1 version
TRACE Found candidate for package aiobotocore with range ==2.21.1 after 1 steps: 2.21.1 version
TRACE Returning candidate for package aiobotocore with range ==2.21.1 after 1 steps
TRACE Returning candidate for package aiobotocore with range ==2.21.1 after 1 steps
DEBUG Selecting: aiobotocore==2.21.1 [compatible] (aiobotocore-2.21.1-py3-none-any.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/95/67/026598918f92145156f2feb7957f57daefda20375cc2ac1a0692a9b8010b/aiobotocore-2.21.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/95/67/026598918f92145156f2feb7957f57daefda20375cc2ac1a0692a9b8010b/aiobotocore-2.21.1-py3-none-any.whl
TRACE Received built distribution metadata for: aiobotocore==2.21.1
DEBUG Adding transitive dependency for aiobotocore==2.21.1: aiohttp>=3.9.2, <4.0.0
DEBUG Adding transitive dependency for aiobotocore==2.21.1: aioitertools>=0.5.1, <1.0.0
DEBUG Adding transitive dependency for aiobotocore==2.21.1: botocore>=1.37.0, <1.37.2
DEBUG Adding transitive dependency for aiobotocore==2.21.1: python-dateutil>=2.1, <3.0.0
DEBUG Adding transitive dependency for aiobotocore==2.21.1: jmespath>=0.7.1, <2.0.0
DEBUG Adding transitive dependency for aiobotocore==2.21.1: multidict>=6.0.0, <7.0.0
DEBUG Adding transitive dependency for aiobotocore==2.21.1: wrapt>=1.10.10, <2.0.0
TRACE Fetching metadata for aiohttp from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1
TRACE Chose package for decision: aiobotocore[boto3]. remaining choices: aiofiles, wrapt, aiohttp, aioitertools, botocore, python-dateutil, jmespath, multidict
DEBUG Searching for a compatible version of aiobotocore[boto3] (==2.21.1)
TRACE Selecting candidate for aiobotocore with range ==2.21.1 with 110 remote versions
TRACE Found candidate for package aiobotocore with range ==2.21.1 after 1 steps: 2.21.1 version
TRACE Returning candidate for package aiobotocore with range ==2.21.1 after 1 steps
TRACE Fetching metadata for aioitertools from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
DEBUG Selecting: aiobotocore==2.21.1 [compatible] (aiobotocore-2.21.1-py3-none-any.whl)
TRACE Fetching metadata for botocore from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
DEBUG Adding transitive dependency for aiobotocore==2.21.1: boto3>=1.37.0, <1.37.2
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1
TRACE Chose package for decision: aiofiles. remaining choices: boto3, wrapt, aiohttp, aioitertools, botocore, python-dateutil, jmespath, multidict
DEBUG Searching for a compatible version of aiofiles (>=23.2.1)
TRACE Selecting candidate for aiofiles with range >=23.2.1 with 14 remote versions
TRACE Found candidate for package aiofiles with range >=23.2.1 after 1 steps: 24.1.0 version
TRACE Returning candidate for package aiofiles with range >=23.2.1 after 1 steps
DEBUG Selecting: aiofiles==24.1.0 [compatible] (aiofiles-24.1.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0
TRACE Chose package for decision: aiohttp. remaining choices: boto3, wrapt, multidict, aioitertools, botocore, python-dateutil, jmespath
TRACE Fetching metadata for python-dateutil from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
TRACE Fetching metadata for jmespath from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
TRACE Fetching metadata for multidict from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
TRACE Fetching metadata for wrapt from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ already contains username and password
TRACE Fetching metadata for boto3 from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ does not have a fresh cache because its age is 242 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/jmespath/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: jmespath
TRACE Selecting candidate for jmespath with range >=0.7.1, <2.0.0 with 26 remote versions
TRACE Found candidate for package jmespath with range >=0.7.1, <2.0.0 after 1 steps: 1.0.1 version
TRACE Returning candidate for package jmespath with range >=0.7.1, <2.0.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/31/b4/b9b800c45527aadd64d5b442f9b932b00648617eb5d63d2c7a6587b7cafc/jmespath-1.0.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/31/b4/b9b800c45527aadd64d5b442f9b932b00648617eb5d63d2c7a6587b7cafc/jmespath-1.0.1-py3-none-any.whl
TRACE Received built distribution metadata for: jmespath==1.0.1
TRACE Received package metadata for: aioitertools
TRACE Selecting candidate for aioitertools with range >=0.5.1, <1.0.0 with 19 remote versions
TRACE Found candidate for package aioitertools with range >=0.5.1, <1.0.0 after 1 steps: 0.12.0 version
TRACE Returning candidate for package aioitertools with range >=0.5.1, <1.0.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl
TRACE Received built distribution metadata for: aioitertools==0.12.0
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/python-dateutil/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/wrapt/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: python-dateutil
TRACE Selecting candidate for python-dateutil with range >=2.1, <3.0.0 with 27 remote versions
TRACE Found candidate for package python-dateutil with range >=2.1, <3.0.0 after 1 steps: 2.9.0.post0 version
TRACE Returning candidate for package python-dateutil with range >=2.1, <3.0.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/ec/57/56b9bcc3c9c6a792fcbaf139543cee77261f3651ca9da0c93f5c1221264b/python_dateutil-2.9.0.post0-py2.py3-none-any.whl
TRACE Received built distribution metadata for: python-dateutil==2.9.0.post0
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/multidict/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/botocore/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/boto3/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ already contains username and password
TRACE Received package metadata for: wrapt
TRACE Selecting candidate for wrapt with range >=1.10.10, <2.0.0 with 59 remote versions
TRACE Found candidate for package wrapt with range >=1.10.10, <2.0.0 after 1 steps: 1.17.2 version
TRACE Returning candidate for package wrapt with range >=1.10.10, <2.0.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/2a/5a/04cde32b07a7431d4ed0553a76fdb7a61270e78c5fd5a603e190ac389f14/wrapt-1.17.2-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/2a/5a/04cde32b07a7431d4ed0553a76fdb7a61270e78c5fd5a603e190ac389f14/wrapt-1.17.2-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Received built distribution metadata for: wrapt==1.17.2
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohttp/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: botocore
TRACE Selecting candidate for botocore with range >=1.37.0, <1.37.2 with 2181 remote versions
TRACE Found candidate for package botocore with range >=1.37.0, <1.37.2 after 1 steps: 1.37.1 version
TRACE Returning candidate for package botocore with range >=1.37.0, <1.37.2 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/3d/20/352b2bf99f93ba18986615841786cbd0d38f7856bd49d4e154a540f04afe/botocore-1.37.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/3d/20/352b2bf99f93ba18986615841786cbd0d38f7856bd49d4e154a540f04afe/botocore-1.37.1-py3-none-any.whl
TRACE Received built distribution metadata for: botocore==1.37.1
TRACE Received package metadata for: multidict
TRACE Selecting candidate for multidict with range >=6.0.0, <7.0.0 with 139 remote versions
TRACE Found candidate for package multidict with range >=6.0.0, <7.0.0 after 1 steps: 6.4.3 version
TRACE Returning candidate for package multidict with range >=6.0.0, <7.0.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/f0/ac/7ced59dcdfeddd03e601edb05adff0c66d81ed4a5160c443e44f2379eef0/multidict-6.4.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/f0/ac/7ced59dcdfeddd03e601edb05adff0c66d81ed4a5160c443e44f2379eef0/multidict-6.4.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Received built distribution metadata for: multidict==6.4.3
TRACE Received package metadata for: boto3
TRACE Selecting candidate for boto3 with range >=1.37.0, <1.37.2 with 1783 remote versions
TRACE Found candidate for package boto3 with range >=1.37.0, <1.37.2 after 1 steps: 1.37.1 version
TRACE Returning candidate for package boto3 with range >=1.37.0, <1.37.2 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/63/ec/e722c53c9dc41e8df094587c32e19409bace8b43b5eb31fe3536ca57a38b/boto3-1.37.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/63/ec/e722c53c9dc41e8df094587c32e19409bace8b43b5eb31fe3536ca57a38b/boto3-1.37.1-py3-none-any.whl
TRACE Received built distribution metadata for: boto3==1.37.1
TRACE Received package metadata for: aiohttp
TRACE Selecting candidate for aiohttp with range >=3.9.2, <4.0.0 with 280 remote versions
DEBUG Searching for a compatible version of aiohttp (>=3.9.2, <4.0.0)
TRACE Selecting candidate for aiohttp with range >=3.9.2, <4.0.0 with 280 remote versions
TRACE Found candidate for package aiohttp with range >=3.9.2, <4.0.0 after 1 steps: 3.11.18 version
TRACE Found candidate for package aiohttp with range >=3.9.2, <4.0.0 after 1 steps: 3.11.18 version
TRACE Returning candidate for package aiohttp with range >=3.9.2, <4.0.0 after 1 steps
TRACE Returning candidate for package aiohttp with range >=3.9.2, <4.0.0 after 1 steps
DEBUG Selecting: aiohttp==3.11.18 [compatible] (aiohttp-3.11.18-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/23/06/4203ffa2beb5bedb07f0da0f79b7d9039d1c33f522e0d1a2d5b6218e6f2e/aiohttp-3.11.18-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/23/06/4203ffa2beb5bedb07f0da0f79b7d9039d1c33f522e0d1a2d5b6218e6f2e/aiohttp-3.11.18-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Received built distribution metadata for: aiohttp==3.11.18
DEBUG Adding transitive dependency for aiohttp==3.11.18: aiohappyeyeballs>=2.3.0
DEBUG Adding transitive dependency for aiohttp==3.11.18: aiosignal>=1.1.2
DEBUG Adding transitive dependency for aiohttp==3.11.18: attrs>=17.3.0
DEBUG Adding transitive dependency for aiohttp==3.11.18: frozenlist>=1.1.1
DEBUG Adding transitive dependency for aiohttp==3.11.18: multidict>=4.5, <7.0
DEBUG Adding transitive dependency for aiohttp==3.11.18: propcache>=0.2.0
DEBUG Adding transitive dependency for aiohttp==3.11.18: yarl>=1.17.0, <2.0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18
TRACE Chose package for decision: aioitertools. remaining choices: boto3, wrapt, multidict, yarl, botocore, python-dateutil, jmespath, aiohappyeyeballs, aiosignal, attrs, frozenlist, propcache
DEBUG Searching for a compatible version of aioitertools (>=0.5.1, <1.0.0)
TRACE Selecting candidate for aioitertools with range >=0.5.1, <1.0.0 with 19 remote versions
TRACE Found candidate for package aioitertools with range >=0.5.1, <1.0.0 after 1 steps: 0.12.0 version
TRACE Returning candidate for package aioitertools with range >=0.5.1, <1.0.0 after 1 steps
DEBUG Selecting: aioitertools==0.12.0 [compatible] (aioitertools-0.12.0-py3-none-any.whl)
TRACE Fetching metadata for aiohappyeyeballs from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0
TRACE Chose package for decision: botocore. remaining choices: boto3, wrapt, multidict, yarl, propcache, python-dateutil, jmespath, aiohappyeyeballs, aiosignal, attrs, frozenlist
DEBUG Searching for a compatible version of botocore (>=1.37.0, <1.37.2)
TRACE Selecting candidate for botocore with range >=1.37.0, <1.37.2 with 2181 remote versions
TRACE Fetching metadata for aiosignal from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
TRACE Found candidate for package botocore with range >=1.37.0, <1.37.2 after 1 steps: 1.37.1 version
TRACE Returning candidate for package botocore with range >=1.37.0, <1.37.2 after 1 steps
DEBUG Selecting: botocore==1.37.1 [compatible] (botocore-1.37.1-py3-none-any.whl)
DEBUG Adding transitive dependency for botocore==1.37.1: jmespath>=0.7.1, <2.0.0
DEBUG Adding transitive dependency for botocore==1.37.1: python-dateutil>=2.1, <3.0.0
DEBUG Adding transitive dependency for botocore==1.37.1: urllib3{python_full_version >= '3.10'}>=1.25.4, <2.2.0 | >=2.2.0+, <3
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1
TRACE Chose package for decision: python-dateutil. remaining choices: boto3, wrapt, multidict, yarl, propcache, jmespath, aiohappyeyeballs, aiosignal, attrs, frozenlist
DEBUG Searching for a compatible version of python-dateutil (>=2.1, <3.0.0)
TRACE Selecting candidate for python-dateutil with range >=2.1, <3.0.0 with 27 remote versions
TRACE Found candidate for package python-dateutil with range >=2.1, <3.0.0 after 1 steps: 2.9.0.post0 version
TRACE Returning candidate for package python-dateutil with range >=2.1, <3.0.0 after 1 steps
TRACE Fetching metadata for attrs from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
DEBUG Selecting: python-dateutil==2.9.0.post0 [compatible] (python_dateutil-2.9.0.post0-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for python-dateutil==2.9.0.post0: six>=1.5
TRACE Fetching metadata for frozenlist from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0
TRACE Chose package for decision: jmespath. remaining choices: boto3, wrapt, multidict, yarl, propcache, six, aiohappyeyeballs, aiosignal, attrs, frozenlist
DEBUG Searching for a compatible version of jmespath (>=0.7.1, <2.0.0)
TRACE Fetching metadata for propcache from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
TRACE Selecting candidate for jmespath with range >=0.7.1, <2.0.0 with 26 remote versions
TRACE Found candidate for package jmespath with range >=0.7.1, <2.0.0 after 1 steps: 1.0.1 version
TRACE Returning candidate for package jmespath with range >=0.7.1, <2.0.0 after 1 steps
DEBUG Selecting: jmespath==1.0.1 [compatible] (jmespath-1.0.1-py3-none-any.whl)
TRACE Fetching metadata for yarl from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1
TRACE Chose package for decision: multidict. remaining choices: boto3, wrapt, frozenlist, yarl, propcache, six, aiohappyeyeballs, aiosignal, attrs
DEBUG Searching for a compatible version of multidict (>=6.0.0, <7.0.0)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ is storable because its response has an 'max-age' cache-control directive
TRACE Selecting candidate for multidict with range >=6.0.0, <7.0.0 with 139 remote versions
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Found candidate for package multidict with range >=6.0.0, <7.0.0 after 1 steps: 6.4.3 version
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ has a cached response that does not allow staleness
TRACE Returning candidate for package multidict with range >=6.0.0, <7.0.0 after 1 steps
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Selecting: multidict==6.4.3 [compatible] (multidict-6.4.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ already contains username and password
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ is storable because its response has an 'max-age' cache-control directive
TRACE Chose package for decision: wrapt. remaining choices: boto3, attrs, frozenlist, yarl, propcache, six, aiohappyeyeballs, aiosignal
TRACE Freshness lifetime found via cache-control max age setting: 60s
DEBUG Searching for a compatible version of wrapt (>=1.10.10, <2.0.0)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ has a cached response that does not allow staleness
TRACE Selecting candidate for wrapt with range >=1.10.10, <2.0.0 with 59 remote versions
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
TRACE Found candidate for package wrapt with range >=1.10.10, <2.0.0 after 1 steps: 1.17.2 version
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
TRACE Returning candidate for package wrapt with range >=1.10.10, <2.0.0 after 1 steps
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
DEBUG Selecting: wrapt==1.17.2 [compatible] (wrapt-1.17.2-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ already contains username and password
TRACE Fetching metadata for urllib3 from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Fetching metadata for six from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2
TRACE Chose package for decision: boto3. remaining choices: aiosignal, attrs, frozenlist, yarl, propcache, six, aiohappyeyeballs
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ is storable because its response has an 'max-age' cache-control directive
DEBUG Searching for a compatible version of boto3 (>=1.37.0, <1.37.2)
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Selecting candidate for boto3 with range >=1.37.0, <1.37.2 with 1783 remote versions
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
TRACE Found candidate for package boto3 with range >=1.37.0, <1.37.2 after 1 steps: 1.37.1 version
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
TRACE Returning candidate for package boto3 with range >=1.37.0, <1.37.2 after 1 steps
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
DEBUG Selecting: boto3==1.37.1 [compatible] (boto3-1.37.1-py3-none-any.whl)
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ already contains username and password
DEBUG Adding transitive dependency for boto3==1.37.1: botocore>=1.37.1, <1.38.0
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ is storable because its response has an 'max-age' cache-control directive
DEBUG Adding transitive dependency for boto3==1.37.1: jmespath>=0.7.1, <2.0.0
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Adding transitive dependency for boto3==1.37.1: s3transfer>=0.11.0, <0.12.0
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ already contains username and password
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1
TRACE Chose package for decision: aiohappyeyeballs. remaining choices: aiosignal, attrs, frozenlist, yarl, propcache, six, s3transfer
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ already contains username and password
TRACE Fetching metadata for s3transfer from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ already contains username and password
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ is storable because its response has an 'max-age' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ has a cached response that does not allow staleness
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ does not have a fresh cache because its age is 243 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/attrs/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiosignal/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: aiosignal
TRACE Selecting candidate for aiosignal with range >=1.1.2 with 9 remote versions
TRACE Found candidate for package aiosignal with range >=1.1.2 after 1 steps: 1.3.2 version
TRACE Returning candidate for package aiosignal with range >=1.1.2 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/ec/6a/bc7e17a3e87a2985d3e8f4da4cd0f481060eb78fb08596c42be62c90a4d9/aiosignal-1.3.2-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/ec/6a/bc7e17a3e87a2985d3e8f4da4cd0f481060eb78fb08596c42be62c90a4d9/aiosignal-1.3.2-py2.py3-none-any.whl
TRACE Received built distribution metadata for: aiosignal==1.3.2
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/s3transfer/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/six/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: six
TRACE Selecting candidate for six with range >=1.5 with 29 remote versions
TRACE Found candidate for package six with range >=1.5 after 1 steps: 1.17.0 version
TRACE Returning candidate for package six with range >=1.5 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/b7/ce/149a00dd41f10bc29e5921b496af8b574d8413afcd5e30dfa0ed46c2cc5e/six-1.17.0-py2.py3-none-any.whl
TRACE Received built distribution metadata for: six==1.17.0
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aiohappyeyeballs/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/urllib3/ is storable because its response has an 'max-age' cache-control directive
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/propcache/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: attrs
TRACE Selecting candidate for attrs with range >=17.3.0 with 34 remote versions
TRACE Found candidate for package attrs with range >=17.3.0 after 1 steps: 25.3.0 version
TRACE Returning candidate for package attrs with range >=17.3.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/77/06/bb80f5f86020c4551da315d78b3ab75e8228f89f0162f2c3a819e407941a/attrs-25.3.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/77/06/bb80f5f86020c4551da315d78b3ab75e8228f89f0162f2c3a819e407941a/attrs-25.3.0-py3-none-any.whl
TRACE Received built distribution metadata for: attrs==25.3.0
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ already contains username and password
TRACE Received package metadata for: s3transfer
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.12.0 with 51 remote versions
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.12.0 after 1 steps: 0.11.5 version
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.12.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/45/39/13402e323666d17850eca87e4cd6ecfcf9fd7809cac9efdcce10272fc29d/s3transfer-0.11.5-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/45/39/13402e323666d17850eca87e4cd6ecfcf9fd7809cac9efdcce10272fc29d/s3transfer-0.11.5-py3-none-any.whl
TRACE Received built distribution metadata for: s3transfer==0.11.5
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/frozenlist/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: aiohappyeyeballs
TRACE Selecting candidate for aiohappyeyeballs with range >=2.3.0 with 44 remote versions
TRACE Found candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps: 2.6.1 version
TRACE Returning candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps
DEBUG Searching for a compatible version of aiohappyeyeballs (>=2.3.0)
TRACE Selecting candidate for aiohappyeyeballs with range >=2.3.0 with 44 remote versions
TRACE Found candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps: 2.6.1 version
TRACE Returning candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps
DEBUG Selecting: aiohappyeyeballs==2.6.1 [compatible] (aiohappyeyeballs-2.6.1-py3-none-any.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/0f/15/5bf3b99495fb160b63f95972b81750f18f7f4e02ad051373b669d17d44f2/aiohappyeyeballs-2.6.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/0f/15/5bf3b99495fb160b63f95972b81750f18f7f4e02ad051373b669d17d44f2/aiohappyeyeballs-2.6.1-py3-none-any.whl
TRACE Received built distribution metadata for: aiohappyeyeballs==2.6.1
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1
TRACE Chose package for decision: aiosignal. remaining choices: s3transfer, attrs, frozenlist, yarl, propcache, six
DEBUG Searching for a compatible version of aiosignal (>=1.1.2)
TRACE Selecting candidate for aiosignal with range >=1.1.2 with 9 remote versions
TRACE Found candidate for package aiosignal with range >=1.1.2 after 1 steps: 1.3.2 version
TRACE Returning candidate for package aiosignal with range >=1.1.2 after 1 steps
DEBUG Selecting: aiosignal==1.3.2 [compatible] (aiosignal-1.3.2-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for aiosignal==1.3.2: frozenlist>=1.1.0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2
TRACE Chose package for decision: attrs. remaining choices: s3transfer, six, frozenlist, yarl, propcache
DEBUG Searching for a compatible version of attrs (>=17.3.0)
TRACE Selecting candidate for attrs with range >=17.3.0 with 34 remote versions
TRACE Found candidate for package attrs with range >=17.3.0 after 1 steps: 25.3.0 version
TRACE Returning candidate for package attrs with range >=17.3.0 after 1 steps
DEBUG Selecting: attrs==25.3.0 [compatible] (attrs-25.3.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0
TRACE Chose package for decision: frozenlist. remaining choices: s3transfer, six, yarl, propcache
TRACE Received package metadata for: urllib3
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/yarl/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: propcache
TRACE Selecting candidate for propcache with range >=0.2.0 with 7 remote versions
TRACE Found candidate for package propcache with range >=0.2.0 after 1 steps: 0.3.1 version
TRACE Returning candidate for package propcache with range >=0.2.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/3b/4c/f72c9e1022b3b043ec7dc475a0f405d4c3e10b9b1d378a7330fecf0652da/propcache-0.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/3b/4c/f72c9e1022b3b043ec7dc475a0f405d4c3e10b9b1d378a7330fecf0652da/propcache-0.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Received built distribution metadata for: propcache==0.3.1
TRACE Received package metadata for: frozenlist
TRACE Selecting candidate for frozenlist with range >=1.1.1 with 14 remote versions
DEBUG Searching for a compatible version of frozenlist (>=1.1.1)
TRACE Selecting candidate for frozenlist with range >=1.1.1 with 14 remote versions
TRACE Found candidate for package frozenlist with range >=1.1.1 after 1 steps: 1.6.0 version
TRACE Returning candidate for package frozenlist with range >=1.1.1 after 1 steps
TRACE Found candidate for package frozenlist with range >=1.1.1 after 1 steps: 1.6.0 version
TRACE Returning candidate for package frozenlist with range >=1.1.1 after 1 steps
DEBUG Selecting: frozenlist==1.6.0 [compatible] (frozenlist-1.6.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/2b/a6/564ecde55ee633270a793999ef4fd1d2c2b32b5a7eec903b1012cb7c5143/frozenlist-1.6.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/2b/a6/564ecde55ee633270a793999ef4fd1d2c2b32b5a7eec903b1012cb7c5143/frozenlist-1.6.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Received built distribution metadata for: frozenlist==1.6.0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0
TRACE Chose package for decision: propcache. remaining choices: s3transfer, six, yarl
DEBUG Searching for a compatible version of propcache (>=0.2.0)
TRACE Selecting candidate for propcache with range >=0.2.0 with 7 remote versions
TRACE Found candidate for package propcache with range >=0.2.0 after 1 steps: 0.3.1 version
TRACE Returning candidate for package propcache with range >=0.2.0 after 1 steps
DEBUG Selecting: propcache==0.3.1 [compatible] (propcache-0.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1
TRACE Chose package for decision: yarl. remaining choices: s3transfer, six
TRACE Received package metadata for: yarl
TRACE Selecting candidate for yarl with range >=1.17.0, <2.0 with 128 remote versions
TRACE Found candidate for package yarl with range >=1.17.0, <2.0 after 1 steps: 1.20.0 version
TRACE Returning candidate for package yarl with range >=1.17.0, <2.0 after 1 steps
DEBUG Searching for a compatible version of yarl (>=1.17.0, <2.0)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/15/45/212604d3142d84b4065d5f8cab6582ed3d78e4cc250568ef2a36fe1cf0a5/yarl-1.20.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
TRACE Selecting candidate for yarl with range >=1.17.0, <2.0 with 128 remote versions
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/15/45/212604d3142d84b4065d5f8cab6582ed3d78e4cc250568ef2a36fe1cf0a5/yarl-1.20.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
TRACE Found candidate for package yarl with range >=1.17.0, <2.0 after 1 steps: 1.20.0 version
TRACE Returning candidate for package yarl with range >=1.17.0, <2.0 after 1 steps
DEBUG Selecting: yarl==1.20.0 [compatible] (yarl-1.20.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Received built distribution metadata for: yarl==1.20.0
DEBUG Adding transitive dependency for yarl==1.20.0: idna>=2.0
DEBUG Adding transitive dependency for yarl==1.20.0: multidict>=4.0
DEBUG Adding transitive dependency for yarl==1.20.0: propcache>=0.2.1
TRACE Fetching metadata for idna from https://foobar%40foobar.com:token@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0
TRACE Chose package for decision: urllib3{python_full_version >= '3.10'}. remaining choices: s3transfer, six, idna
DEBUG Searching for a compatible version of urllib3{python_full_version >= '3.10'} (>=1.25.4, <2.2.0 | >=2.2.0+, <3)
TRACE Selecting candidate for urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 with 98 remote versions
TRACE Found candidate for package urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for urllib3==2.4.0: urllib3==2.4.0
DEBUG Adding transitive dependency for urllib3==2.4.0: urllib3{python_full_version >= '3.10'}==2.4.0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0
TRACE Chose package for decision: urllib3. remaining choices: s3transfer, six, idna, urllib3{python_full_version >= '3.10'}
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ is storable because its response has an 'max-age' cache-control directive
DEBUG Searching for a compatible version of urllib3 (==2.4.0)
TRACE Freshness lifetime found via cache-control max age setting: 60s
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ has a cached response that does not allow staleness
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ does not have a fresh cache because its age is 244 seconds, it is greater than the freshness lifetime of 60 seconds and stale cached responses are not allowed
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
DEBUG Found stale response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
DEBUG Sending revalidation request for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ already contains username and password
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/6b/11/cc635220681e93a0183390e26485430ca2c7b5f9d33b15c74c2861cb8091/urllib3-2.4.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/6b/11/cc635220681e93a0183390e26485430ca2c7b5f9d33b15c74c2861cb8091/urllib3-2.4.0-py3-none-any.whl
TRACE Received built distribution metadata for: urllib3==2.4.0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, urllib3==2.4.0
TRACE Chose package for decision: urllib3{python_full_version >= '3.10'}. remaining choices: s3transfer, six, idna
DEBUG Searching for a compatible version of urllib3{python_full_version >= '3.10'} (==2.4.0)
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0
TRACE Chose package for decision: six. remaining choices: s3transfer, idna
DEBUG Searching for a compatible version of six (>=1.5)
TRACE Selecting candidate for six with range >=1.5 with 29 remote versions
TRACE Found candidate for package six with range >=1.5 after 1 steps: 1.17.0 version
TRACE Returning candidate for package six with range >=1.5 after 1 steps
DEBUG Selecting: six==1.17.0 [compatible] (six-1.17.0-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, six==1.17.0
TRACE Chose package for decision: s3transfer. remaining choices: idna
DEBUG Searching for a compatible version of s3transfer (>=0.11.0, <0.12.0)
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.12.0 with 51 remote versions
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.12.0 after 1 steps: 0.11.5 version
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.12.0 after 1 steps
DEBUG Selecting: s3transfer==0.11.5 [compatible] (s3transfer-0.11.5-py3-none-any.whl)
DEBUG Adding transitive dependency for s3transfer==0.11.5: botocore>=1.37.4, <2.0a0
DEBUG Recording unit propagation conflict of s3transfer from incompatibility of (s3transfer, botocore)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1
TRACE Chose package for decision: urllib3{python_full_version >= '3.10'}. remaining choices: idna, s3transfer, six, aiohappyeyeballs, yarl, propcache, frozenlist, attrs, aiosignal
DEBUG Searching for a compatible version of urllib3{python_full_version >= '3.10'} (>=1.25.4, <2.2.0 | >=2.2.0+, <3)
TRACE Selecting candidate for urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 with 98 remote versions
TRACE Found candidate for package urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range >=1.25.4, <2.2.0 | >=2.2.0+, <3 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1
TRACE Chose package for decision: urllib3. remaining choices: idna, s3transfer, six, aiohappyeyeballs, yarl, propcache, frozenlist, attrs, aiosignal, urllib3{python_full_version >= '3.10'}
DEBUG Searching for a compatible version of urllib3 (==2.4.0)
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 with 51 remote versions
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0
TRACE Chose package for decision: urllib3{python_full_version >= '3.10'}. remaining choices: idna, s3transfer, six, aiohappyeyeballs, yarl, propcache, frozenlist, attrs, aiosignal
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 after 2 steps: 0.11.4 version
DEBUG Searching for a compatible version of urllib3{python_full_version >= '3.10'} (==2.4.0)
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 after 2 steps
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
DEBUG Selecting: urllib3==2.4.0 [compatible] (urllib3-2.4.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0
TRACE Chose package for decision: aiohappyeyeballs. remaining choices: idna, s3transfer, six, aiosignal, yarl, propcache, frozenlist, attrs
DEBUG Searching for a compatible version of aiohappyeyeballs (>=2.3.0)
TRACE Selecting candidate for aiohappyeyeballs with range >=2.3.0 with 44 remote versions
TRACE Found candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps: 2.6.1 version
TRACE Returning candidate for package aiohappyeyeballs with range >=2.3.0 after 1 steps
DEBUG Selecting: aiohappyeyeballs==2.6.1 [compatible] (aiohappyeyeballs-2.6.1-py3-none-any.whl)
TRACE Selecting candidate for urllib3 with range ==2.4.0 with 98 remote versions
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1
TRACE Found candidate for package urllib3 with range ==2.4.0 after 1 steps: 2.4.0 version
TRACE Returning candidate for package urllib3 with range ==2.4.0 after 1 steps
TRACE Chose package for decision: aiosignal. remaining choices: idna, s3transfer, six, attrs, yarl, propcache, frozenlist
DEBUG Searching for a compatible version of aiosignal (>=1.1.2)
TRACE Selecting candidate for aiosignal with range >=1.1.2 with 9 remote versions
TRACE Found candidate for package aiosignal with range >=1.1.2 after 1 steps: 1.3.2 version
TRACE Returning candidate for package aiosignal with range >=1.1.2 after 1 steps
DEBUG Selecting: aiosignal==1.3.2 [compatible] (aiosignal-1.3.2-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2
TRACE Chose package for decision: attrs. remaining choices: idna, s3transfer, six, frozenlist, yarl, propcache
DEBUG Searching for a compatible version of attrs (>=17.3.0)
TRACE Selecting candidate for attrs with range >=17.3.0 with 34 remote versions
TRACE Found candidate for package attrs with range >=17.3.0 after 1 steps: 25.3.0 version
TRACE Returning candidate for package attrs with range >=17.3.0 after 1 steps
DEBUG Selecting: attrs==25.3.0 [compatible] (attrs-25.3.0-py3-none-any.whl)
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/86/62/8d3fc3ec6640161a5649b2cddbbf2b9fa39c92541225b33f117c37c5a2eb/s3transfer-0.11.4-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
TRACE Chose package for decision: frozenlist. remaining choices: idna, s3transfer, six, propcache, yarl
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/86/62/8d3fc3ec6640161a5649b2cddbbf2b9fa39c92541225b33f117c37c5a2eb/s3transfer-0.11.4-py3-none-any.whl
DEBUG Searching for a compatible version of frozenlist (>=1.1.1)
TRACE Selecting candidate for frozenlist with range >=1.1.1 with 14 remote versions
TRACE Found candidate for package frozenlist with range >=1.1.1 after 1 steps: 1.6.0 version
TRACE Returning candidate for package frozenlist with range >=1.1.1 after 1 steps
DEBUG Selecting: frozenlist==1.6.0 [compatible] (frozenlist-1.6.0-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0
TRACE Chose package for decision: propcache. remaining choices: idna, s3transfer, six, yarl
DEBUG Searching for a compatible version of propcache (>=0.2.0)
TRACE Selecting candidate for propcache with range >=0.2.0 with 7 remote versions
TRACE Received built distribution metadata for: s3transfer==0.11.4
TRACE Found candidate for package propcache with range >=0.2.0 after 1 steps: 0.3.1 version
TRACE Returning candidate for package propcache with range >=0.2.0 after 1 steps
DEBUG Selecting: propcache==0.3.1 [compatible] (propcache-0.3.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1
TRACE Chose package for decision: yarl. remaining choices: idna, s3transfer, six
DEBUG Searching for a compatible version of yarl (>=1.17.0, <2.0)
TRACE Selecting candidate for yarl with range >=1.17.0, <2.0 with 128 remote versions
TRACE Found candidate for package yarl with range >=1.17.0, <2.0 after 1 steps: 1.20.0 version
TRACE Returning candidate for package yarl with range >=1.17.0, <2.0 after 1 steps
DEBUG Selecting: yarl==1.20.0 [compatible] (yarl-1.20.0-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0
TRACE Chose package for decision: six. remaining choices: idna, s3transfer
DEBUG Searching for a compatible version of six (>=1.5)
TRACE Selecting candidate for six with range >=1.5 with 29 remote versions
TRACE Found candidate for package six with range >=1.5 after 1 steps: 1.17.0 version
TRACE Returning candidate for package six with range >=1.5 after 1 steps
DEBUG Selecting: six==1.17.0 [compatible] (six-1.17.0-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, six==1.17.0
TRACE Chose package for decision: s3transfer. remaining choices: idna
DEBUG Searching for a compatible version of s3transfer (>=0.11.0, <0.11.5 | >0.11.5, <0.12.0)
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 with 51 remote versions
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 after 2 steps: 0.11.4 version
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.11.5 | >0.11.5, <0.12.0 after 2 steps
DEBUG Selecting: s3transfer==0.11.4 [compatible] (s3transfer-0.11.4-py3-none-any.whl)
DEBUG Adding transitive dependency for s3transfer==0.11.4: botocore>=1.37.4, <2.0a0
DEBUG Recording dependency conflict of s3transfer==0.11.4 from incompatibility of (s3transfer, botocore)
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, six==1.17.0
TRACE Chose package for decision: s3transfer. remaining choices: idna
DEBUG Searching for a compatible version of s3transfer (>=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0)
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 with 51 remote versions
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 after 3 steps: 0.11.3 version
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 after 3 steps
DEBUG Selecting: s3transfer==0.11.3 [compatible] (s3transfer-0.11.3-py3-none-any.whl)
TRACE Selecting candidate for s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 with 51 remote versions
TRACE Found candidate for package s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 after 3 steps: 0.11.3 version
TRACE Returning candidate for package s3transfer with range >=0.11.0, <0.11.4 | >0.11.4, <0.11.5 | >0.11.5, <0.12.0 after 3 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/e4/81/48c41b554a54d75d4407740abb60e3a102ae416284df04d1dbdcbe3dbf24/s3transfer-0.11.3-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/e4/81/48c41b554a54d75d4407740abb60e3a102ae416284df04d1dbdcbe3dbf24/s3transfer-0.11.3-py3-none-any.whl
TRACE Received built distribution metadata for: s3transfer==0.11.3
DEBUG Adding transitive dependency for s3transfer==0.11.3: botocore>=1.36.0, <2.0a0
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, six==1.17.0, s3transfer==0.11.3
TRACE Chose package for decision: idna. remaining choices: 
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
DEBUG Found modified response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/
TRACE Handling request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ with authentication policy auto
TRACE Request for https://foobar%40foobar.com:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ already contains username and password
TRACE Updating cached credentials for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ to Basic { username: Username(Some("foobar@foobar.com")), password: Some(****) }
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/idna/ is storable because its response has an 'max-age' cache-control directive
TRACE Received package metadata for: idna
TRACE Selecting candidate for idna with range >=2.0 with 32 remote versions
DEBUG Searching for a compatible version of idna (>=2.0)
TRACE Found candidate for package idna with range >=2.0 after 1 steps: 3.10 version
TRACE Selecting candidate for idna with range >=2.0 with 32 remote versions
TRACE Returning candidate for package idna with range >=2.0 after 1 steps
TRACE Found candidate for package idna with range >=2.0 after 1 steps: 3.10 version
TRACE Returning candidate for package idna with range >=2.0 after 1 steps
DEBUG Selecting: idna==3.10 [compatible] (idna-3.10-py3-none-any.whl)
TRACE Selecting candidate for idna with range >=2.0 with 32 remote versions
TRACE Found candidate for package idna with range >=2.0 after 1 steps: 3.10 version
TRACE Returning candidate for package idna with range >=2.0 after 1 steps
TRACE Cached request https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 31536000s
DEBUG Found fresh response for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/packages/packages/76/c6/c88e154df9c4e1a2a66ccf0005a88dfb2650c1dffb6f5ce603dfbd452ce3/idna-3.10-py3-none-any.whl
TRACE Received built distribution metadata for: idna==3.10
TRACE Assigned packages: root==0a0.dev0, aioboto3==14.1.0, aiobotocore==2.21.1, aiobotocore[boto3]==2.21.1, aiofiles==24.1.0, aiohttp==3.11.18, aioitertools==0.12.0, botocore==1.37.1, python-dateutil==2.9.0.post0, jmespath==1.0.1, multidict==6.4.3, wrapt==1.17.2, boto3==1.37.1, urllib3==2.4.0, urllib3{python_full_version >= '3.10'}==2.4.0, aiohappyeyeballs==2.6.1, aiosignal==1.3.2, attrs==25.3.0, frozenlist==1.6.0, propcache==0.3.1, yarl==1.20.0, six==1.17.0, s3transfer==0.11.3, idna==3.10
DEBUG Tried 23 versions: s3transfer 3, aioboto3 1, aiobotocore 1, aiofiles 1, aiohappyeyeballs 1, aiohttp 1, aioitertools 1, aiosignal 1, attrs 1, boto3 1, botocore 1, frozenlist 1, idna 1, jmespath 1, multidict 1, propcache 1, python-dateutil 1, six 1, urllib3 1, wrapt 1, yarl 1
DEBUG marker environment resolution took 4.194s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.8", version: "3.12.8" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.12.23-1-lts", platform_system: "Linux", platform_version: "#1 SMP PREEMPT_DYNAMIC Thu, 10 Apr 2025 13:28:36 +0000", python_full_version: StringVersion { string: "3.12.8", version: "3.12.8" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> aioboto3
TRACE Resolution edge:     0a0.dev0 -> 14.1.0
TRACE Resolution edge: aiohttp -> aiohappyeyeballs
TRACE Resolution edge:     3.11.18 -> 2.6.1
TRACE Resolution edge: aiohttp -> aiosignal
TRACE Resolution edge:     3.11.18 -> 1.3.2
TRACE Resolution edge: aiohttp -> attrs
TRACE Resolution edge:     3.11.18 -> 25.3.0
TRACE Resolution edge: aiohttp -> frozenlist
TRACE Resolution edge:     3.11.18 -> 1.6.0
TRACE Resolution edge: aiohttp -> multidict
TRACE Resolution edge:     3.11.18 -> 6.4.3
TRACE Resolution edge: aiohttp -> propcache
TRACE Resolution edge:     3.11.18 -> 0.3.1
TRACE Resolution edge: aiohttp -> yarl
TRACE Resolution edge:     3.11.18 -> 1.20.0
TRACE Resolution edge: s3transfer -> botocore
TRACE Resolution edge:     0.11.3 -> 1.37.1
TRACE Resolution edge: python-dateutil -> six
TRACE Resolution edge:     2.9.0.post0 -> 1.17.0
TRACE Resolution edge: aioboto3 -> aiobotocore
TRACE Resolution edge:     14.1.0 -> 2.21.1 (extra: boto3)
TRACE Resolution edge: aioboto3 -> aiobotocore
TRACE Resolution edge:     14.1.0 -> 2.21.1
TRACE Resolution edge: aioboto3 -> aiofiles
TRACE Resolution edge:     14.1.0 -> 24.1.0
TRACE Resolution edge: aiobotocore -> aiohttp
TRACE Resolution edge:     2.21.1 -> 3.11.18
TRACE Resolution edge: aiobotocore -> aioitertools
TRACE Resolution edge:     2.21.1 -> 0.12.0
TRACE Resolution edge: aiobotocore -> botocore
TRACE Resolution edge:     2.21.1 -> 1.37.1
TRACE Resolution edge: aiobotocore -> python-dateutil
TRACE Resolution edge:     2.21.1 -> 2.9.0.post0
TRACE Resolution edge: aiobotocore -> jmespath
TRACE Resolution edge:     2.21.1 -> 1.0.1
TRACE Resolution edge: aiobotocore -> multidict
TRACE Resolution edge:     2.21.1 -> 6.4.3
TRACE Resolution edge: aiobotocore -> wrapt
TRACE Resolution edge:     2.21.1 -> 1.17.2
TRACE Resolution edge: aiosignal -> frozenlist
TRACE Resolution edge:     1.3.2 -> 1.6.0
TRACE Resolution edge: boto3 -> botocore
TRACE Resolution edge:     1.37.1 -> 1.37.1
TRACE Resolution edge: boto3 -> jmespath
TRACE Resolution edge:     1.37.1 -> 1.0.1
TRACE Resolution edge: boto3 -> s3transfer
TRACE Resolution edge:     1.37.1 -> 0.11.3
TRACE Resolution edge: aiobotocore -> boto3
TRACE Resolution edge:     2.21.1 (extra: boto3) -> 1.37.1
TRACE Resolution edge: botocore -> jmespath
TRACE Resolution edge:     1.37.1 -> 1.0.1
TRACE Resolution edge: botocore -> python-dateutil
TRACE Resolution edge:     1.37.1 -> 2.9.0.post0
TRACE Resolution edge: botocore -> urllib3
TRACE Resolution edge:     1.37.1 -> 2.4.0 ; python_full_version >= '3.10'
TRACE Resolution edge: yarl -> idna
TRACE Resolution edge:     1.20.0 -> 3.10
TRACE Resolution edge: yarl -> multidict
TRACE Resolution edge:     1.20.0 -> 6.4.3
TRACE Resolution edge: yarl -> propcache
TRACE Resolution edge:     1.20.0 -> 0.3.1
Resolved 21 packages in 4.19s
DEBUG Registry requirement already cached: propcache==0.3.1
DEBUG Registry requirement already cached: frozenlist==1.6.0
DEBUG Registry requirement already cached: aiofiles==24.1.0
DEBUG Registry requirement already cached: jmespath==1.0.1
DEBUG Registry requirement already cached: attrs==25.3.0
DEBUG Registry requirement already cached: aiobotocore==2.21.1
DEBUG Registry requirement already cached: six==1.17.0
DEBUG Registry requirement already cached: yarl==1.20.0
DEBUG Registry requirement already cached: aioboto3==14.1.0
DEBUG Registry requirement already cached: python-dateutil==2.9.0.post0
DEBUG Registry requirement already cached: s3transfer==0.11.3
DEBUG Registry requirement already cached: aiohappyeyeballs==2.6.1
DEBUG Registry requirement already cached: boto3==1.37.1
DEBUG Registry requirement already cached: urllib3==2.4.0
DEBUG Registry requirement already cached: wrapt==1.17.2
DEBUG Registry requirement already cached: botocore==1.37.1
DEBUG Registry requirement already cached: multidict==6.4.3
DEBUG Registry requirement already cached: aiosignal==1.3.2
DEBUG Registry requirement already cached: aiohttp==3.11.18
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: aioitertools==0.12.0
TRACE Extracting file name=PackageName("s3transfer")
TRACE Extracting file name=PackageName("propcache")
TRACE Extracting file name=PackageName("aiofiles")
TRACE Extracting file name=PackageName("jmespath")
TRACE Extracting file name=PackageName("frozenlist")
TRACE Extracting file name=PackageName("aiobotocore")
TRACE Extracting file name=PackageName("yarl")
TRACE Extracted 13 files name=PackageName("propcache")
TRACE Extracted 11 files name=PackageName("frozenlist")
TRACE Extracted 22 files name=PackageName("s3transfer")
TRACE No entrypoints name=PackageName("propcache")
TRACE Extracted 14 files name=PackageName("jmespath")
TRACE No entrypoints name=PackageName("s3transfer")
TRACE No data name=PackageName("propcache")
TRACE No entrypoints name=PackageName("frozenlist")
TRACE Writing installer metadata name=PackageName("propcache")
TRACE No data name=PackageName("s3transfer")
TRACE Writing installer metadata name=PackageName("s3transfer")
TRACE No data name=PackageName("frozenlist")
TRACE Writing installer metadata name=PackageName("frozenlist")
TRACE Extracted 15 files name=PackageName("aiofiles")
TRACE Extracted 17 files name=PackageName("yarl")
TRACE No entrypoints name=PackageName("jmespath")
TRACE Installing data/scripts dist_name=PackageName("jmespath")
TRACE No entrypoints name=PackageName("aiofiles")
TRACE No data name=PackageName("aiofiles")
TRACE Writing installer metadata name=PackageName("aiofiles")
TRACE No entrypoints name=PackageName("yarl")
TRACE Writing record name=PackageName("frozenlist")
TRACE Writing record name=PackageName("propcache")
TRACE Writing record name=PackageName("s3transfer")
TRACE No data name=PackageName("yarl")
TRACE Writing installer metadata name=PackageName("yarl")
TRACE Extracted 37 files name=PackageName("aiobotocore")
TRACE Writing record name=PackageName("aiofiles")
TRACE Writing installer metadata name=PackageName("jmespath")
TRACE Extracting file name=PackageName("aiohappyeyeballs")
TRACE No entrypoints name=PackageName("aiobotocore")
TRACE Writing record name=PackageName("yarl")
TRACE No data name=PackageName("aiobotocore")
TRACE Writing installer metadata name=PackageName("aiobotocore")
TRACE Extracting file name=PackageName("botocore")
TRACE Writing record name=PackageName("jmespath")
TRACE Extracting file name=PackageName("attrs")
TRACE Extracting file name=PackageName("aioboto3")
TRACE Writing record name=PackageName("aiobotocore")
TRACE Extracting file name=PackageName("boto3")
TRACE Extracted 10 files name=PackageName("aiohappyeyeballs")
TRACE Extracting file name=PackageName("six")
TRACE No entrypoints name=PackageName("aiohappyeyeballs")
TRACE No data name=PackageName("aiohappyeyeballs")
TRACE Writing installer metadata name=PackageName("aiohappyeyeballs")
TRACE Writing record name=PackageName("aiohappyeyeballs")
TRACE Extracted 6 files name=PackageName("six")
TRACE No entrypoints name=PackageName("six")
TRACE No data name=PackageName("six")
TRACE Writing installer metadata name=PackageName("six")
TRACE Extracting file name=PackageName("python-dateutil")
TRACE Writing record name=PackageName("six")
TRACE Extracted 35 files name=PackageName("attrs")
TRACE Extracted 21 files name=PackageName("aioboto3")
TRACE No entrypoints name=PackageName("aioboto3")
TRACE No data name=PackageName("aioboto3")
TRACE No entrypoints name=PackageName("attrs")
TRACE Writing installer metadata name=PackageName("aioboto3")
TRACE No data name=PackageName("attrs")
TRACE Writing installer metadata name=PackageName("attrs")
TRACE Extracting file name=PackageName("urllib3")
TRACE Writing record name=PackageName("attrs")
TRACE Writing record name=PackageName("aioboto3")
TRACE Extracting file name=PackageName("wrapt")
TRACE Extracting file name=PackageName("aiohttp")
TRACE Extracted 25 files name=PackageName("python-dateutil")
TRACE No entrypoints name=PackageName("python-dateutil")
TRACE No data name=PackageName("python-dateutil")
TRACE Writing installer metadata name=PackageName("python-dateutil")
TRACE Extracted 14 files name=PackageName("wrapt")
TRACE Writing record name=PackageName("python-dateutil")
TRACE No entrypoints name=PackageName("wrapt")
TRACE No data name=PackageName("wrapt")
TRACE Writing installer metadata name=PackageName("wrapt")
TRACE Extracting file name=PackageName("idna")
TRACE Writing record name=PackageName("wrapt")
TRACE Extracting file name=PackageName("aioitertools")
TRACE Extracted 42 files name=PackageName("urllib3")
TRACE No entrypoints name=PackageName("urllib3")
TRACE Extracted 13 files name=PackageName("idna")
TRACE Extracted 63 files name=PackageName("boto3")
TRACE Extracting file name=PackageName("multidict")
TRACE No entrypoints name=PackageName("idna")
TRACE Extracting file name=PackageName("aiosignal")
TRACE No data name=PackageName("urllib3")
TRACE Writing installer metadata name=PackageName("urllib3")
TRACE No data name=PackageName("idna")
TRACE Writing installer metadata name=PackageName("idna")
TRACE No entrypoints name=PackageName("boto3")
TRACE No data name=PackageName("boto3")
TRACE Writing installer metadata name=PackageName("boto3")
TRACE Writing record name=PackageName("urllib3")
TRACE Writing record name=PackageName("idna")
TRACE Writing record name=PackageName("boto3")
TRACE Extracted 20 files name=PackageName("aioitertools")
TRACE No entrypoints name=PackageName("aioitertools")
TRACE No data name=PackageName("aioitertools")
TRACE Writing installer metadata name=PackageName("aioitertools")
TRACE Extracted 78 files name=PackageName("aiohttp")
TRACE Extracted 8 files name=PackageName("aiosignal")
TRACE No entrypoints name=PackageName("aiohttp")
TRACE No data name=PackageName("aiohttp")
TRACE Extracted 11 files name=PackageName("multidict")
TRACE Writing installer metadata name=PackageName("aiohttp")
TRACE Writing record name=PackageName("aioitertools")
TRACE No entrypoints name=PackageName("aiosignal")
TRACE No data name=PackageName("aiosignal")
TRACE Writing installer metadata name=PackageName("aiosignal")
TRACE No entrypoints name=PackageName("multidict")
TRACE No data name=PackageName("multidict")
TRACE Writing installer metadata name=PackageName("multidict")
TRACE Writing record name=PackageName("aiohttp")
TRACE Writing record name=PackageName("aiosignal")
TRACE Writing record name=PackageName("multidict")
TRACE Extracted 1840 files name=PackageName("botocore")
TRACE No entrypoints name=PackageName("botocore")
TRACE No data name=PackageName("botocore")
TRACE Writing installer metadata name=PackageName("botocore")
TRACE Writing record name=PackageName("botocore")
Installed 21 packages in 66ms
 + aioboto3==14.1.0
 + aiobotocore==2.21.1
 + aiofiles==24.1.0
 + aiohappyeyeballs==2.6.1
 + aiohttp==3.11.18
 + aioitertools==0.12.0
 + aiosignal==1.3.2
 + attrs==25.3.0
 + boto3==1.37.1
 + botocore==1.37.1
 + frozenlist==1.6.0
 + idna==3.10
 + jmespath==1.0.1
 + multidict==6.4.3
 + propcache==0.3.1
 + python-dateutil==2.9.0.post0
 + s3transfer==0.11.3
 + six==1.17.0
 + urllib3==2.4.0
 + wrapt==1.17.2
 + yarl==1.20.0
DEBUG Released lock at `/home/foobar/work/foobar-service/.venv/.lock`

```

---

_Comment by @hwong557 on 2025-05-01 18:51_

I'm not an admin on our company's jfrog repo and am unaware of how it's configured but I can ask around.

---

_Comment by @jtfmumm on 2025-05-01 18:58_

Thank you! Were the failing trace logs above also from a local run? 

---

_Comment by @jtfmumm on 2025-05-01 19:00_

And if not, would you mind sharing the local logs for the failure case?

---

_Comment by @hwong557 on 2025-05-01 19:13_

The failing trace logs were from cicd, not from local.

Interestingly, 0.7.{0,2} work for me locally but not in CI 🤷. The mystery deepens. Sorry for testing your patience.  Let me spend a little time getting obfuscated passing/failing trace logs in the same environment. Thank you for looking at this.

---

_Comment by @zanieb on 2025-05-01 19:35_

It seems plausible that in CI/CD the credentials are not working and it was falling back to PyPI? which is the very thing #12805 is intended to prevent.

---

_Comment by @desmondcheongzx on 2025-05-01 21:55_

FWIW, when I use `--extra-index-url https://d1p3klp2t5517h.cloudfront.net/builds/nightly`, the given url does not need credentials.

Here are our logs:
```
Last login: Thu May  1 14:53:57 on ttys009
; cd misc
; source .venv/bin/activate
(.venv) ; uv pip install -r requirements-dev.txt daft --pre --extra-index-url https://d1p3klp2t5517h.cloudfront.net/builds/nightly --force-reinstall -vv
DEBUG uv 0.7.2 (481d05d8d 2025-04-30)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /Users/desmond/misc/.venv/bin/python3
DEBUG Found `cpython-3.12.9-macos-aarch64-none` at `/Users/desmond/misc/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: daft*
DEBUG Adding direct dependency: ipdb*
DEBUG Adding direct dependency: maturin*
DEBUG Adding direct dependency: pre-commit*
DEBUG Adding direct dependency: docker*
DEBUG Adding direct dependency: importlib-metadata*
DEBUG Adding direct dependency: requests<2.32.0
DEBUG Adding direct dependency: httpx>=0.27.2, <0.27.2+
DEBUG Adding direct dependency: orjson>=3.10.12, <3.10.12+
DEBUG Adding direct dependency: py-spy>=0.3.14, <0.3.14+
DEBUG Adding direct dependency: viztracer>=0.15.6, <0.15.6+
DEBUG Adding direct dependency: hypothesis>=6.79.2, <6.79.2+
DEBUG Adding direct dependency: pytest>=7.4.3, <7.4.3+
DEBUG Adding direct dependency: pytest-benchmark>=4.0.0, <4.0.0+
DEBUG Adding direct dependency: pytest-cov>=4.1.0, <4.1.0+
DEBUG Adding direct dependency: pytest-lazy-fixture>=0.6.3, <0.6.3+
DEBUG Adding direct dependency: memray{sys_platform != 'win32'}>=1.13.4, <1.13.4+
DEBUG Adding direct dependency: pytest-codspeed>=2.2.1, <2.2.1+
DEBUG Adding direct dependency: lxml>=5.3.0, <5.3.0+
DEBUG Adding direct dependency: dask[dataframe]>=2024.4.1, <2024.4.1+
DEBUG Adding direct dependency: numpy>=1.26.2, <1.26.2+
DEBUG Adding direct dependency: pandas>=2.1.3, <2.1.3+
DEBUG Adding direct dependency: xxhash>=3.0.0
DEBUG Adding direct dependency: pillow>=10.4.0, <10.4.0+
DEBUG Adding direct dependency: opencv-python>=4.10.0.84, <4.10.0.84+
DEBUG Adding direct dependency: tiktoken>=0.7.0, <0.7.0+
DEBUG Adding direct dependency: duckdb>=1.1.2, <1.1.2+
DEBUG Adding direct dependency: tqdm*
DEBUG Adding direct dependency: pyarrow>=19.0.0, <19.0.0+
DEBUG Adding direct dependency: ray[data]>=2.34.0, <2.34.0+
DEBUG Adding direct dependency: ray[client]>=2.34.0, <2.34.0+
DEBUG Adding direct dependency: pylance>=0.20.0
DEBUG Adding direct dependency: pyiceberg>=0.7.0, <0.7.0+
DEBUG Adding direct dependency: pydantic>=2.10.6, <2.10.6+
DEBUG Adding direct dependency: tenacity>=8.2.3, <8.2.3+
DEBUG Adding direct dependency: deltalake{sys_platform != 'win32'}>=0.19.2, <0.19.2+
DEBUG Adding direct dependency: databricks-sdk>=0.12.0, <0.12.0+
DEBUG Adding direct dependency: unitycatalog>=0.1.1, <0.1.1+
DEBUG Adding direct dependency: sqlalchemy>=2.0.36, <2.0.36+
DEBUG Adding direct dependency: connectorx{platform_machine != 'aarch64' or sys_platform != 'linux'}>=0.3.3, <0.3.3+
DEBUG Adding direct dependency: trino[sqlalchemy]>=0.328.0, <0.328.0+
DEBUG Adding direct dependency: pymysql>=1.1.0, <1.1.0+
DEBUG Adding direct dependency: psycopg2-binary>=2.9.10, <2.9.10+
DEBUG Adding direct dependency: sqlglot>=23.3.0, <23.3.0+
DEBUG Adding direct dependency: pyodbc>=5.1.0, <5.1.0+
DEBUG Adding direct dependency: s3fs>=2023.12.0, <2023.12.0+
DEBUG Adding direct dependency: boto3>=1.36.20, <1.36.20+
DEBUG Adding direct dependency: boto3-stubs[essential]>=1.36.20, <1.36.20+
DEBUG Adding direct dependency: boto3-stubs[glue]>=1.36.20, <1.36.20+
DEBUG Adding direct dependency: boto3-stubs[s3]>=1.36.20, <1.36.20+
DEBUG Adding direct dependency: boto3-stubs[s3tables]>=1.36.20, <1.36.20+
DEBUG Adding direct dependency: moto[glue]>=5.1.1, <5.1.1+
DEBUG Adding direct dependency: moto[s3]>=5.1.1, <5.1.1+
DEBUG Adding direct dependency: moto[s3tables]>=5.1.1, <5.1.1+
DEBUG Adding direct dependency: moto[server]>=5.1.1, <5.1.1+
DEBUG Adding direct dependency: adlfs>=2024.7.0, <2024.7.0+
DEBUG Adding direct dependency: azure-storage-blob>=12.24.0, <12.24.0+
DEBUG Adding direct dependency: gcsfs>=2023.12.0, <2023.12.0+
DEBUG Adding direct dependency: markdown-exec*
DEBUG Adding direct dependency: mkdocs-jupyter*
DEBUG Adding direct dependency: mkdocs-material*
DEBUG Adding direct dependency: mkdocs-macros-plugin*
DEBUG Adding direct dependency: mkdocs-simple-hooks*
DEBUG Adding direct dependency: pymdown-extensions*
DEBUG Adding direct dependency: mkdocs-material[imaging]*
DEBUG Adding direct dependency: mkdocstrings-python*
DEBUG Adding direct dependency: ruff*
DEBUG Adding direct dependency: mkdocs-minify-plugin*
DEBUG Adding direct dependency: pyspark>=3.5.3, <3.5.3+
DEBUG Adding direct dependency: grpcio>=1.68.1, <1.68.1+
DEBUG Adding direct dependency: grpcio-status>=1.67.0, <1.67.0+
TRACE Fetching metadata for daft from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Assigned packages: root==0a0.dev0
TRACE Fetching metadata for ipdb from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/
TRACE Chose package for decision: httpx. remaining choices: daft, ipdb, maturin, pre-commit, docker, importlib-metadata, requests, grpcio-status, orjson, py-spy, viztracer, hypothesis, pytest, pytest-benchmark, pytest-cov, pytest-lazy-fixture, pytest-codspeed, lxml, numpy, pandas, xxhash, pillow, opencv-python, tiktoken, duckdb, tqdm, pyarrow, pylance, pyiceberg, pydantic, tenacity, databricks-sdk, unitycatalog, sqlalchemy, pymysql, psycopg2-binary, sqlglot, pyodbc, s3fs, boto3, adlfs, azure-storage-blob, gcsfs, markdown-exec, mkdocs-jupyter, mkdocs-material, mkdocs-macros-plugin, mkdocs-simple-hooks, pymdown-extensions, mkdocstrings-python, ruff, mkdocs-minify-plugin, pyspark, grpcio
TRACE Fetching metadata for maturin from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/
TRACE Fetching metadata for pre-commit from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/
TRACE Fetching metadata for docker from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/
TRACE Fetching metadata for importlib-metadata from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/
TRACE Fetching metadata for requests from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/
TRACE Fetching metadata for httpx from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/
TRACE Fetching metadata for orjson from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/
TRACE Fetching metadata for py-spy from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/
TRACE Fetching metadata for viztracer from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/
TRACE Fetching metadata for hypothesis from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/
TRACE Fetching metadata for pytest from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/
TRACE Fetching metadata for pytest-benchmark from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/
TRACE Fetching metadata for pytest-cov from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/
TRACE Fetching metadata for pytest-lazy-fixture from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/
TRACE Fetching metadata for memray from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/
TRACE Fetching metadata for pytest-codspeed from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/
TRACE Fetching metadata for lxml from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/
TRACE Fetching metadata for dask from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/
TRACE Fetching metadata for numpy from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/
TRACE Fetching metadata for pandas from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/
TRACE Fetching metadata for xxhash from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/
TRACE Fetching metadata for pillow from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/
TRACE Fetching metadata for opencv-python from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/
TRACE Fetching metadata for tiktoken from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/
TRACE Fetching metadata for duckdb from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/
TRACE Fetching metadata for tqdm from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/
TRACE Fetching metadata for pyarrow from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/
TRACE Fetching metadata for ray from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/
TRACE Fetching metadata for pylance from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/
TRACE Fetching metadata for pyiceberg from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/
TRACE Fetching metadata for pydantic from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/
TRACE Fetching metadata for tenacity from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/
TRACE Fetching metadata for deltalake from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/
TRACE Fetching metadata for databricks-sdk from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/
TRACE Fetching metadata for unitycatalog from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/
TRACE Fetching metadata for sqlalchemy from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/
TRACE Fetching metadata for connectorx from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/
TRACE Fetching metadata for trino from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/
TRACE Fetching metadata for pymysql from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/
TRACE Fetching metadata for psycopg2-binary from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/
TRACE Fetching metadata for sqlglot from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/
TRACE Fetching metadata for pyodbc from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/
TRACE Fetching metadata for s3fs from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/
TRACE Fetching metadata for boto3 from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/
TRACE Fetching metadata for boto3-stubs from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/
TRACE Fetching metadata for moto from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/
TRACE Fetching metadata for adlfs from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/
TRACE Fetching metadata for azure-storage-blob from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/
TRACE Cached request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ is storable because its response has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ does not have a fresh cache because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
DEBUG Sending revalidation request for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/maturin.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pre-commit.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/docker.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/requests.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/ipdb.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/orjson.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/importlib-metadata.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/httpx.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/viztracer.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/hypothesis.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/py-spy.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pytest.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pytest-benchmark.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pytest-cov.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pytest-lazy-fixture.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/memray.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pytest-codspeed.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/lxml.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/dask.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/numpy.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pandas.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/xxhash.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pillow.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/opencv-python.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/tiktoken.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/duckdb.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/tqdm.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pyarrow.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/ray.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pylance.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pyiceberg.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pydantic.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/tenacity.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/deltalake.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/databricks-sdk.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/unitycatalog.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/sqlalchemy.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/connectorx.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/trino.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pymysql.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/psycopg2-binary.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/sqlglot.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pyodbc.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/s3fs.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/boto3.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/boto3-stubs.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/moto.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/adlfs.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/azure-storage-blob.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/
DEBUG Found modified response for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Cached request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ is not storable because its response has unsupported status code 304
WARN Server returned unusable 304 for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/
TRACE Cached request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: daft
TRACE Fetching metadata for gcsfs from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/
TRACE Selecting candidate for daft with range * with 15 remote versions
TRACE Found candidate for package daft with range * after 1 steps: 0.4.13.dev2+g70a76dbf version
TRACE Returning candidate for package daft with range * after 1 steps
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/gcsfs.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
DEBUG No netrc file found
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/requests/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/requests/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: requests
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/maturin/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/maturin/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: maturin
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pytest-lazy-fixture/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-lazy-fixture/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pytest-lazy-fixture
TRACE Fetching metadata for markdown-exec from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/
TRACE Fetching metadata for mkdocs-jupyter from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/
TRACE Fetching metadata for mkdocs-material from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/markdown-exec.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocs-jupyter.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocs-material.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/hypothesis/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/hypothesis/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: hypothesis
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pytest-cov/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-cov/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pytest-cov
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/tqdm/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tqdm/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: tqdm
TRACE Fetching metadata for mkdocs-macros-plugin from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/
TRACE Fetching metadata for mkdocs-simple-hooks from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/
TRACE Fetching metadata for pymdown-extensions from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocs-macros-plugin.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocs-simple-hooks.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pymdown-extensions.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/ray/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ray/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: ray
TRACE Fetching metadata for mkdocstrings-python from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocstrings-python.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/numpy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/numpy/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: numpy
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/viztracer/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/viztracer/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: viztracer
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/py-spy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/py-spy/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: py-spy
TRACE Fetching metadata for ruff from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/
TRACE Fetching metadata for mkdocs-minify-plugin from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/
TRACE Fetching metadata for pyspark from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/ruff.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/mkdocs-minify-plugin.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/pyspark.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/opencv-python/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/opencv-python/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: opencv-python
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pandas/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pandas/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pandas
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pre-commit/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pre-commit/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pre-commit
TRACE Fetching metadata for grpcio from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/
TRACE Fetching metadata for grpcio-status from https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/grpcio.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/
TRACE No cache entry exists for /Users/desmond/Library/Caches/uv/simple-v15/index/b0f7dc2e2b961dea/grpcio-status.rkyv
DEBUG No cache entry for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/
TRACE Sending fresh GET request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/ with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/
TRACE Cached request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl is storable because its response has a heuristically cacheable status code 200
TRACE Could not determine freshness lifetime, assuming none exists
TRACE Request https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl does not have a fresh cache because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl
DEBUG Sending revalidation request for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl
TRACE Handling request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl with authentication policy auto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl
TRACE Attempting unauthenticated request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/dask/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/dask/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: dask
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/duckdb/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/duckdb/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: duckdb
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/tenacity/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tenacity/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: tenacity
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pymysql/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymysql/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pymysql
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/s3fs/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/s3fs/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: s3fs
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/unitycatalog/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/unitycatalog/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: unitycatalog
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pyiceberg/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyiceberg/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pyiceberg
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/importlib-metadata/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/importlib-metadata/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: importlib-metadata
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/xxhash/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/xxhash/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: xxhash
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/tiktoken/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/tiktoken/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: tiktoken
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/boto3-stubs/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3-stubs/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: boto3-stubs
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/deltalake/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/deltalake/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: deltalake
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/sqlalchemy/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlalchemy/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: sqlalchemy
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pyodbc/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyodbc/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pyodbc
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/boto3/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/boto3/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: boto3
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/lxml/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/lxml/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: lxml
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/orjson/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/orjson/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: orjson
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pytest-benchmark/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-benchmark/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pytest-benchmark
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/ipdb/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ipdb/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: ipdb
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/docker/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/docker/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: docker
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pytest-codspeed/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest-codspeed/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pytest-codspeed
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/memray/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/memray/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: memray
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/azure-storage-blob/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/azure-storage-blob/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: azure-storage-blob
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/trino/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/trino/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: trino
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pyarrow/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyarrow/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pyarrow
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/databricks-sdk/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/databricks-sdk/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: databricks-sdk
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pydantic/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pydantic/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pydantic
TRACE Resource is not modified because old and new etag values ([34, 54, 48, 54, 57, 49, 100, 101, 53, 55, 98, 97, 97, 98, 57, 57, 52, 100, 56, 56, 48, 100, 101, 100, 100, 99, 101, 51, 99, 54, 53, 101, 57, 45, 53, 34]) match
DEBUG Found not-modified response for: https://d1p3klp2t5517h.cloudfront.net/builds/nightly/daft-0.4.13.dev2%2Bg70a76dbf-cp39-abi3-macosx_11_0_arm64.whl
TRACE Received built distribution metadata for: daft==0.4.13.dev2+g70a76dbf
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pytest/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pytest/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pytest
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pylance/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pylance/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pylance
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/sqlglot/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/sqlglot/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: sqlglot
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/adlfs/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/adlfs/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: adlfs
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/connectorx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/connectorx/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: connectorx
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/httpx/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/httpx/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: httpx
DEBUG Searching for a compatible version of httpx (>=0.27.2, <0.27.2+)
TRACE Selecting candidate for httpx with range >=0.27.2, <0.27.2+ with 0 remote versions
DEBUG No compatible version found for: httpx
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/psycopg2-binary/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/psycopg2-binary/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: psycopg2-binary
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pillow/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pillow/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pillow
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/moto/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/moto/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: moto
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/gcsfs/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/gcsfs/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: gcsfs
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/markdown-exec/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/markdown-exec/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: markdown-exec
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pymdown-extensions/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pymdown-extensions/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pymdown-extensions
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocs-material/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-material/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocs-material
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocs-minify-plugin/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-minify-plugin/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocs-minify-plugin
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocs-macros-plugin/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-macros-plugin/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocs-macros-plugin
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocs-simple-hooks/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-simple-hooks/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocs-simple-hooks
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/ruff/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/ruff/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: ruff
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/pyspark/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/pyspark/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: pyspark
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocstrings-python/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocstrings-python/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocstrings-python
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/grpcio-status/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio-status/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: grpcio-status
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/grpcio/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/grpcio/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: grpcio
TRACE Request for https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/ failed with 403 Forbidden, checking for credentials
TRACE No credentials in cache for URL https://d1p3klp2t5517h.cloudfront.net/builds/nightly
TRACE Skipping fetch of credentials for https://d1p3klp2t5517h.cloudfront.net/builds/nightly, previous attempt failed
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("d1p3klp2t5517h.cloudfront.net")), port: None, path: "/builds/nightly/mkdocs-jupyter/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: mkdocs-jupyter
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on httpx==0.27.2
  httpx not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on httpx==0.27.2
  httpx not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because httpx was not found in the package registry and you require
      httpx==0.27.2, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://d1p3klp2t5517h.cloudfront.net/builds/nightly)
      could not be queried due to a lack of valid authentication credentials
      (403 Forbidden).
DEBUG Released lock at `/Users/desmond/misc/.venv/.lock`
```

---

_Comment by @desmondcheongzx on 2025-05-01 22:08_

Also fwiw, here's a clueless person's stab at understanding what's happening :)

We do `uv pip install -r requirements-dev.txt daft`. We want `daft` to come from our nightly build which lives at `https://d1p3klp2t5517h.cloudfront.net/builds/nightly`, but we want the rest of the requirements to come from pypi.

One of the dev requirements for example is `mkdocs-jupyter`, so uv tries to search for `https://d1p3klp2t5517h.cloudfront.net/builds/nightly/mkdocs-jupyter/`.

This URL does not exist. But because our objects are stored in S3 buckets, you get a 403 instead of a 404 since S3 doesn't reveal whether the object exists or not unless we give it permission to do so.

Since it's a 403 instead of a 404, uv stops searching indexes---by design it seems, if I understand #12805 correctly.

I guess everything is working by design. Is there a way to specify `ignore-error-codes` on the command line? This part to me is unclear.

---

_Referenced in [Eventual-Inc/Daft#4289](../../Eventual-Inc/Daft/pulls/4289.md) on 2025-05-02 08:01_

---

_Comment by @jtfmumm on 2025-05-02 08:32_

Yes, your interpretation sounds likely. At the moment, the only way to ignore authentication failures is by configuring the index in `pyproject.toml`.

---

_Comment by @hwong557 on 2025-05-04 14:55_

Here are passing/failing logs from the same environment. We attempt to install a single package `aioitertools`.

<details><summary>Passing logs</summary>
<p>

```
DEBUG uv 0.6.17
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /codefresh/volume/foobar-service/.venv/bin/python3
DEBUG Found `cpython-3.12.10-linux-x86_64-gnu` at `/codefresh/volume/foobar-service/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.10 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: aioitertools
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.10
DEBUG Solving with target Python version: >=3.12.10
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: aioitertools*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: aioitertools. remaining choices: 
TRACE Fetching metadata for aioitertools from https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE No cache entry exists for /root/.cache/uv/simple-v15/index/7166f2fb68f9a269/aioitertools.rkyv
DEBUG No cache entry for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Handling request for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ with authentication policy auto
TRACE Request for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foobar.jfrog.io")), port: None, path: "/artifactory/api/pypi/pypi/simple/aioitertools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for aioitertools from https://pypi.org/simple/aioitertools/
TRACE No cache entry exists for /root/.cache/uv/simple-v15/pypi/aioitertools.rkyv
DEBUG No cache entry for: https://pypi.org/simple/aioitertools/
TRACE Sending fresh GET request for https://pypi.org/simple/aioitertools/
TRACE Handling request for https://pypi.org/simple/aioitertools/ with authentication policy auto
TRACE Request for https://pypi.org/simple/aioitertools/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/aioitertools/
TRACE Attempting unauthenticated request for https://pypi.org/simple/aioitertools/
TRACE Cached request https://pypi.org/simple/aioitertools/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: aioitertools
TRACE Selecting candidate for aioitertools with range * with 19 remote versions
TRACE Found candidate for package aioitertools with range * after 1 steps: 0.12.0 version
TRACE Returning candidate for package aioitertools with range * after 1 steps
DEBUG Searching for a compatible version of aioitertools (*)
TRACE Selecting candidate for aioitertools with range * with 19 remote versions
TRACE Found candidate for package aioitertools with range * after 1 steps: 0.12.0 version
TRACE Returning candidate for package aioitertools with range * after 1 steps
DEBUG Selecting: aioitertools==0.12.0 [compatible] (aioitertools-0.12.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v5/pypi/aioitertools/0.12.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata
TRACE Cached request https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: aioitertools==0.12.0
TRACE Assigned packages: root==0a0.dev0, aioitertools==0.12.0
DEBUG Tried 1 versions: aioitertools 1
DEBUG marker environment resolution took 0.094s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.10", version: "3.12.10" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "5.10.230-223.885.amzn2.x86_64", platform_system: "Linux", platform_version: "#1 SMP Tue Dec 3 14:36:00 UTC 2024", python_full_version: StringVersion { string: "3.12.10", version: "3.12.10" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "linux" } }) } }
TRACE Resolution edge: ROOT -> aioitertools
TRACE Resolution edge:     0a0.dev0 -> 0.12.0
Resolved 1 package in 95ms
DEBUG Identified uncached distribution: aioitertools==0.12.0
TRACE No cache entry exists for /root/.cache/uv/wheels-v5/pypi/aioitertools/0.12.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl
TRACE Cached request https://files.pythonhosted.org/packages/85/13/58b70a580de00893223d61de8fea167877a3aed97d4a5e1405c9159ef925/aioitertools-0.12.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
Prepared 1 package in 10ms
TRACE Extracting file name=PackageName("aioitertools")
DEBUG Failed to hardlink `/codefresh/volume/foobar-service/.venv/lib/python3.12/site-packages/aioitertools/more_itertools.py` to `/root/.cache/uv/archive-v0/DhLhGayDc6C_bvulsGvWa/aioitertools/more_itertools.py`, attempting to copy files as a fallback
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
TRACE Extracted 20 files name=PackageName("aioitertools")
TRACE No entrypoints name=PackageName("aioitertools")
TRACE No data name=PackageName("aioitertools")
TRACE Writing installer metadata name=PackageName("aioitertools")
TRACE Writing record name=PackageName("aioitertools")
Installed 1 package in 5ms
 + aioitertools==0.12.0
DEBUG Released lock at `/codefresh/volume/foobar-service/.venv/.lock`

```


</p>
</details> 


<details><summary>Failing logs</summary>
<p>

```
DEBUG uv 0.7.0
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /codefresh/volume/foobar-service/.venv/bin/python3
DEBUG Found `cpython-3.12.10-linux-x86_64-gnu` at `/codefresh/volume/foobar-service/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.10 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: aioitertools
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple
TRACE Caching credentials for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.10
DEBUG Solving with target Python version: >=3.12.10
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: aioitertools*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: aioitertools. remaining choices: 
TRACE Fetching metadata for aioitertools from https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE No cache entry exists for /root/.cache/uv/simple-v15/index/7166f2fb68f9a269/aioitertools.rkyv
DEBUG No cache entry for: https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Sending fresh GET request for https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/
TRACE Handling request for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ with authentication policy auto
TRACE Request for https://****:****@foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foobar.jfrog.io")), port: None, path: "/artifactory/api/pypi/pypi/simple/aioitertools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: aioitertools
DEBUG Searching for a compatible version of aioitertools (*)
TRACE Selecting candidate for aioitertools with range * with 0 remote versions
DEBUG No compatible version found for: aioitertools
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioitertools*
  aioitertools not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aioitertools*
  aioitertools not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because aioitertools was not found in the package registry and you
      require aioitertools, we can conclude that your requirements are
      unsatisfiable.

      hint: An index URL
      (https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple) could
      not be queried due to a lack of valid authentication credentials (403
      Forbidden).
DEBUG Released lock at `/codefresh/volume/foobar-service/.venv/.lock`

```

</p>
</details> 

The logs between the two start to differ after the line
```
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("foobar.jfrog.io")), port: None, path: "/artifactory/api/pypi/pypi/simple/aioitertools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://foobar.jfrog.io/artifactory/api/pypi/pypi/simple/aioitertools/" }))) }

```

---

_Comment by @jtfmumm on 2025-05-04 18:14_

Thanks for sharing these! This difference is what I would expect to see after the upgrade to 0.7. uv no longer ignores 403s by default. 

The first question is if the credentials have the correct permissions. If they do, then it’s possible a JFrog configuration is causing it to return 403s even when permissions are correct. In that case, you can configure the index in `pyproject.toml` to ignore 403s with `ignore-error-codes = [403]`.

---

_Comment by @m-karo on 2025-05-05 08:59_

Hello. I experiencing issue which seems similar to reported here. In latest versions uv failing when --extra-index-url of private repositories are defined in additional to public pypi under "-i". From trace logs seems it starts requesting all modules from extra index instead main index. If extra index definitions are removed from requirements.txt file then everyhing is fine. 

![Image](https://github.com/user-attachments/assets/ccfc7510-3f45-45d8-89db-1069dcc79513)

---
