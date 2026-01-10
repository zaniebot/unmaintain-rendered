---
number: 11102
title: 401 Unauthorized for artifactory repo when running uv pip install
type: issue
state: closed
author: edeustace
labels:
  - bug
assignees: []
created_at: 2025-01-30T17:06:37Z
updated_at: 2025-01-31T09:54:22Z
url: https://github.com/astral-sh/uv/issues/11102
synced_at: 2026-01-10T01:25:01Z
---

# 401 Unauthorized for artifactory repo when running uv pip install

---

_Issue opened by @edeustace on 2025-01-30 17:06_

### Summary

Hi, 
I'm trying to call a private index and I'm getting a 401.

I've see the other issues related to this here: #1709, #1393 etc, but I'm still seeing the issue even though it's been marked as resolved for #1709.

I'm using a virtual env - could that be the issue?

The same index url works fine w/ a vanilla `python -m pip install ...`.


Command: 
`uv --verbose pip install requests-mock --index-url "https://ed.eustace:PASSWORD_HERE@artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple`

```shell
DEBUG uv 0.5.25 (9c07c3fc5 2025-01-28)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.10.16, skipping probing: .my-env/bin/python3
DEBUG Found `cpython-3.10.16-macos-aarch64-none` at `/Users/ed.eustace/dev/code/my-project/.my-env/bin/python3` (active virtual environment)
Using Python 3.10.16 environment at: .my-env
TRACE Checking lock for `.my-env` at `.my-env/.lock`
DEBUG Acquired lock for `.my-env`
DEBUG At least one requirement is not satisfied: requests-mock
TRACE Caching credentials for https://ed.eustace:PASSWORD_HERE@artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.16
DEBUG Solving with target Python version: >=3.10.16
TRACE assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: requests-mock*
TRACE Fetching metadata for requests-mock from https://ed.eustace:PASSWORD_HERE@artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/
TRACE assigned packages: root==0a0.dev0
TRACE Chose package for decision: requests-mock. remaining choices:
TRACE No cache entry exists for /Users/ed.eustace/.cache/uv/simple-v15/index/9651e5c67f3509dd/requests-mock.rkyv
DEBUG No cache entry for: https://artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/
TRACE Sending fresh GET request for https://artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/
TRACE Handling request for https://ed.eustace:****@artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/
TRACE Request for https://ed.eustace:****@artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/ is already fully authenticated
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("artifactory.foobar.com")), port: None, path: "/artifactory/api/pypi/python-virtual/simple/requests-mock/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple/requests-mock/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Received package metadata for: requests-mock
DEBUG Searching for a compatible version of requests-mock (*)
TRACE Selecting candidate for requests-mock with range * with 0 remote versions
DEBUG No compatible version found for: requests-mock
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on requests-mock*
  requests-mock not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on requests-mock*
  requests-mock not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because requests-mock was not found in the package registry and you require requests-mock, we
      can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://artifactory.foobar.com/artifactory/api/pypi/python-virtual/simple)
      could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
DEBUG Released lock at `/Users/ed.eustace/dev/code/my-project/.my-env/.lock`

```

### Platform

macOS Darwin 24.2.0 arm64

### Version

0.5.25

### Python version

Python 3.10

---

_Label `bug` added by @edeustace on 2025-01-30 17:06_

---

_Comment by @zanieb on 2025-01-30 17:11_

Hm.. I'm not sure what to think of this. We say the request is fully authenticated, so we are sending the username and password. Are you _certain_ the same credentials are being used in pip?

---

_Closed by @zanieb on 2025-01-30 17:11_

---

_Reopened by @zanieb on 2025-01-30 17:11_

---

_Comment by @zanieb on 2025-01-30 17:12_

(bonked the button there)

---

_Comment by @edeustace on 2025-01-30 20:47_

Let me double-double check and report back

---

_Comment by @edeustace on 2025-01-31 09:54_

Ok - I found out what the issue was - I had to use my 'api key' for the password field, not my encrypted password. so : `https://$USER:$API_KEY@artifactory.foobar.com/....`. All good. Thanks.

---

_Closed by @edeustace on 2025-01-31 09:54_

---

_Referenced in [astral-sh/uv#11130](../../astral-sh/uv/issues/11130.md) on 2025-01-31 10:43_

---
