---
number: 13216
title: Using UV_EXTRA_INDEX_URL fails in 0.7, regression from 0.6.17
type: issue
state: closed
author: brynmathias
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-04-30T08:32:15Z
updated_at: 2025-06-16T00:39:27Z
url: https://github.com/astral-sh/uv/issues/13216
synced_at: 2026-01-10T01:25:30Z
---

# Using UV_EXTRA_INDEX_URL fails in 0.7, regression from 0.6.17

---

_Issue opened by @brynmathias on 2025-04-30 08:32_

### Summary

I have an internal facing repository configured with:
UV_EXTRA_INDEX_URL=....

this index doesn't mirror the pypi.org repository

When I try to install packages with UV 0.7 I get an error with "package not found" for packages that don't exist in our index, which is correct
However this error should be ignored by default and the next index checked

this can be fixed by explicitly adding the extra index in the `pyproject.toml`
with the flag
ignore-error-codes = [401]

I would propose that for the env var UV_EXTRA_INDEX_URL that the ignore codes are set to sensible defaults, rather than having to manually configure the repository 



### Platform

macOS, linux build pipelines etc

### Version

uv 0.7

### Python version

3.12

---

_Label `bug` added by @brynmathias on 2025-04-30 08:32_

---

_Comment by @juhaszp95 on 2025-04-30 08:34_

Just flagging my related comment [here](https://github.com/astral-sh/uv/pull/12805#issuecomment-2841198133).

---

_Comment by @jtfmumm on 2025-04-30 08:38_

Does this internal-facing index require authentication?

The reason we no longer ignore 401s is because this is not a secure by default approach to dependency confusion attacks. Imagine a user is looking for a package in the private index (whether specified in `pyproject.toml` or via a command line argument) and there is a malicious package with the same name on PyPI. An authentication failure does not actually tell us that the package is not present in the private index. In effect, if we fall through on 401s, the fact that the private index was secured by authentication has made the user more vulnerable to dependency confusion.

---

_Comment by @brynmathias on 2025-04-30 08:51_

I understand the point @jtfmumm 
What I think is happening is as follows:

1. We auth with the repo and check for the package
2. The public package isn't found in the private repo
3. we retry, but without creds (or we hit the jfrog rate limit for logging in)
4. We then receive a 401
5. install fails

So we need to do something to counter this behaviour, either a wait time between retries from the private index, or we skip it after the initial 404 and move on to the next index and try that

---

_Comment by @brynmathias on 2025-04-30 08:53_

and yes the internal index requires authentication

the specific example I am seeing is:
Internal index -- Non mirroring, hosts 2 packages we need
PyPi -- hosts everything other than the internal packages


UV sync tries to install setup tools, looks in the internal index and doesn't find it, tries again. First try we get a 404, second try we get a 401.
The 404 gets retried on the internal index, the 401 is raised by the index server on the second try

install fails

---

_Comment by @jtfmumm on 2025-04-30 08:59_

Thanks for the context. I can investigate this. I would have expected the initial 404 to lead uv to fall through. Would you mind sharing the trace (`-vv`) logs?

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-30 09:03_

---

_Comment by @jesusfbes on 2025-04-30 09:24_

I have a similar error when using uv in a Azure Pipeline to get a package from azure feed. Going back to previous version works.



---

_Comment by @brynmathias on 2025-04-30 09:24_

```ACE Request for https://ANON_ORG.jfrog.io/setuptools/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for URL https://ANON_ORG.jfrog.io/
DEBUG No netrc file found
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("ANON_ORG.jfrog.io")), port: None, path: "/setuptools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://ANON_ORG.jfrog.io/setuptools/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for setuptools from https://ANON_USER%40ANON_ORG.ai:[scrubbed]@ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE idle interval checking for expired
TRACE No cache entry exists for /Users/ANON_USER/.cache/uv/simple-v15/index/c542f1043ba3dc97/setuptools.rkyv
DEBUG No cache entry for: https://ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE Sending fresh GET request for https://ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE Handling request for https://ANON_USER%40ANON_ORG.ai:****@ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/ with authentication policy auto
TRACE Request for https://ANON_USER%40ANON_ORG.ai:****@ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/ already contains username and password
TRACE take? ("https", ANON_ORG.jfrog.io): expiration = Some(90s)
DEBUG reuse idle connection for ("https", ANON_ORG.jfrog.io)
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("ANON_ORG.jfrog.io")), port: None, path: "/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://ANON_ORG.jfrog.io/artifactory/api/pypi/ANON_ORG-no-pypi/simple/setuptools/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for setuptools from https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE put; add idle connection for ("https", ANON_ORG.jfrog.io)
DEBUG pooling idle connection for ("https", ANON_ORG.jfrog.io)
TRACE No cache entry exists for /Users/ANON_USER/.cache/uv/simple-v15/index/eb4ee5ea165afeaf/setuptools.rkyv
DEBUG No cache entry for: https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE Sending fresh GET request for https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE Handling request for https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/ with authentication policy auto
TRACE Request for https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE Attempting unauthenticated request for https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/
TRACE take? ("https", ANON_ORG.jfrog.io): expiration = Some(90s)
DEBUG reuse idle connection for ("https", ANON_ORG.jfrog.io)
TRACE put; add idle connection for ("https", ANON_ORG.jfrog.io)
DEBUG pooling idle connection for ("https", ANON_ORG.jfrog.io)
TRACE Request for https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for URL https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("ANON_ORG.jfrog.io")), port: None, path: "/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(401), url: "https://ANON_ORG.jfrog.io/ANON_ORG/api/pypi/ANON_ORG-no-pypi/simple/setuptools/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: setuptools
DEBUG Searching for a compatible version of setuptools (*)
TRACE Selecting candidate for setuptools with range * with 0 remote versions
DEBUG No compatible version found for: setuptools```

---

_Comment by @brynmathias on 2025-04-30 09:24_

That's what I've got at the moment, getting one of our devs to grab me full logs @jtfmumm 

---

_Referenced in [astral-sh/uv#12805](../../astral-sh/uv/pulls/12805.md) on 2025-04-30 09:25_

---

_Comment by @jtfmumm on 2025-04-30 10:56_

I'm having trouble reproducing this. Would you mind including the relevant sections of your `pyproject.toml` file and the command you are running that triggers this? And how are you providing credentials for your private index?

---

_Comment by @jc-5s on 2025-04-30 11:32_

just wanted to note that we are experiencing the same issue.  

we pass credentials to uv on the cmd line using --extra-index-url i.e.
pip uv install --extra-index-url https://username:password@url ...


---

_Comment by @jtfmumm on 2025-04-30 12:35_

> just wanted to note that we are experiencing the same issue.
> 
> we pass credentials to uv on the cmd line using --extra-index-url i.e. pip uv install --extra-index-url https://username:password@url ...

Thanks. Would you mind sharing some more details? Which index are you using? How have you configured pyproject.toml? And could you share the trace logs (‘-vv’)?

I’m still unable to reproduce

---

_Comment by @constantinevitt on 2025-04-30 12:42_

I have a similar problem. I have a private pypi server with fallback to the official pypi and with uv version 0.7.0 it stopped being able to find packages not hosted on the private server. I thought I could use `ignore-error-codes = [302]` but that doesn't work either.

---

_Comment by @jtfmumm on 2025-04-30 12:44_

> I have a similar problem. I have a private pypi server with fallback to the official pypi and with uv version 0.7.0 it stopped being able to find packages not hosted on the private server. I thought I could use `ignore-error-codes = [302]` but that doesn't work either.

Is that fixed with 0.7.1? I don’t think it’s related to this

---

_Label `needs-mre` added by @jtfmumm on 2025-04-30 14:44_

---

_Comment by @wep21 on 2025-05-01 01:04_

I've encountered the same problem. I've also tried 0.7.1, but it returns 403.
```
No solution found when resolving dependencies:
  ╰─▶ Because aaa was not found in the package registry and you require aaa, we can conclude that your
      requirements are unsatisfiable.

      hint: An index URL (https://pypi.aaa.dev/) could not be queried due to a lack of valid authentication credentials
      (403 Forbidden).
```
I'm currently using username/password url.

---

_Comment by @jtfmumm on 2025-05-01 07:08_

> I've encountered the same problem. I've also tried 0.7.1, but it returns 403.
> 
> ```
> No solution found when resolving dependencies:
>   ╰─▶ Because aaa was not found in the package registry and you require aaa, we can conclude that your
>       requirements are unsatisfiable.
> 
>       hint: An index URL (https://pypi.aaa.dev/) could not be queried due to a lack of valid authentication credentials
>       (403 Forbidden).
> ```
> 
> I'm currently using username/password url.

That sounds like a distinct problem to me. Would you mind sharing your trace logs?

---

_Comment by @wep21 on 2025-05-01 07:19_

Here is the trace log.
```
DEBUG uv 0.7.1 (Homebrew 2025-04-30)
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.12.9, skipping probing: .venv/bin/python3
DEBUG Found `cpython-3.12.9-macos-aarch64-none` at `/Users/daisuke/workspace/patron-rating-viewer/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.9 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: aaa
TRACE Caching credentials for https://username:password@pypi.aaa.dev/
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.9
DEBUG Solving with target Python version: >=3.12.9
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: aaa*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: aaa. remaining choices: 
TRACE Fetching metadata for aaa from https://username:password@pypi.aaa.dev/aaa/
TRACE Cached request https://pypi.aaa.dev/aaa/ is storable because its response has a heuristically cacheable status code 200
TRACE Could not determine freshness lifetime, assuming none exists
TRACE Cached request https://pypi.aaa.dev/aaa/ has a cached response that does not allow staleness
TRACE Request https://pypi.aaa.dev/aaa/ does not have a fresh cache because its age is 671436 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.aaa.dev/aaa/
DEBUG Sending revalidation request for: https://pypi.aaa.dev/aaa/
TRACE Handling request for https://username:****@pypi.aaa.dev/aaa/ with authentication policy auto
TRACE Request for https://username:****@pypi.aaa.dev/aaa/ already contains username and password
TRACE Considering retry of error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.aaa.dev")), port: None, path: "/aaa/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(403), url: "https://pypi.aaa.dev/simple/aaa/" }))) }
TRACE Cannot retry error: not an IO error
DEBUG Indexes search failed because of status code failure: 403 Forbidden
TRACE Received package metadata for: aaa
DEBUG Searching for a compatible version of aaa (*)
TRACE Selecting candidate for aaa with range * with 0 remote versions
DEBUG No compatible version found for: aaa
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aaa*
  aaa not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on aaa*
  aaa not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because aaa was not found in the package registry and you require aaa, we can conclude that your requirements are
      unsatisfiable.

      hint: An index URL (https://pypi.aaa.dev/) could not be queried due to a lack of valid authentication credentials
      (403 Forbidden).
```

---

_Comment by @jtfmumm on 2025-05-01 07:49_

@wep21 Thanks! This looks like a separate issue. Would you mind opening one so we can explore further?

---

_Comment by @wep21 on 2025-05-01 08:01_

@jtfmumm I'm sorry, this is my fault. When debugging pixi 0.7.0, I've mistakenly removed auth user from pypi server. It works fine with pixi 0.7.1 after adding auth user.

---

_Comment by @constantinevitt on 2025-05-01 15:04_

> Is that fixed with 0.7.1? I don’t think it’s related to this

Indeed, it is fixed in 0.7.1. Thank you for the quick reply.

---

_Comment by @ScottWilliamAnderson on 2025-05-13 15:52_

> > Is that fixed with 0.7.1? I don’t think it’s related to this
> 
> Indeed, it is fixed in 0.7.1. Thank you for the quick reply.

Should this be marked as resolved? or @brynmathias are you still experiencing the issue at the moment with uv > 0.7.1

---

_Comment by @valentinoli on 2025-06-13 20:43_

This is still a problem when authenticating to Azure Artifacts. I keep getting errors in `uv==0.7.13` like the following when installing packages and having a private index URL configured. It works with `uv==0.6.*`

```
  × No solution found when resolving dependencies:
  ╰─▶ Because ruff was not found in the package registry and you require
      ruff==0.11.0, we can conclude that your requirements are unsatisfiable.

      hint: An index URL
      (https://pkgs.dev.azure.com/[**REDACTED**]/[**REDACTED**]/_packaging/[**REDACTED**]/pypi/simple/)
      could not be queried due to a lack of valid authentication credentials
      (401 Unauthorized).
```

---

_Comment by @charliermarsh on 2025-06-13 20:48_

Are you sure that the authentication is actually working on prior uv versions? Or is it just falling through to PyPI? We'd need to see trace (`-vv`) logs to help you out here.

---

_Comment by @valentinoli on 2025-06-15 20:10_

@charliermarsh I revisited this and found out it was my fault, I did not configure the auth token. Sorry for the false alarm.

---

_Comment by @zanieb on 2025-06-16 00:37_

Thanks for following up!

---

_Closed by @zanieb on 2025-06-16 00:37_

---

_Comment by @zanieb on 2025-06-16 00:39_

(to the OP: if your problem persists and you can get full logs, please let us know and we can help. I think these cases are mostly part of the intentional security change in 0.7)

---
