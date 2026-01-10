```yaml
number: 17343
title: "Index: Redirect to url using basic auth with empty username doesn't work"
type: issue
state: open
author: matthuisman
labels:
  - bug
assignees: []
created_at: 2026-01-07T04:29:10Z
updated_at: 2026-01-07T21:18:53Z
url: https://github.com/astral-sh/uv/issues/17343
synced_at: 2026-01-10T03:11:36Z
```

# Index: Redirect to url using basic auth with empty username doesn't work

---

_Issue opened by @matthuisman on 2026-01-07 04:29_

### Summary

Have setup a local server to redirect http://127.0.0.1:8000/{path} -> http://:{jwt}@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/{path}

```
DEBUG No cache entry for: http://127.0.0.1:8000/python-environment/
TRACE Sending fresh GET request for http://127.0.0.1:8000/python-environment/
TRACE Handling request for http://127.0.0.1:8000/python-environment/ with authentication policy auto
TRACE Request for http://127.0.0.1:8000/python-environment/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://127.0.0.1:8000/python-environment/
TRACE Attempting unauthenticated request for http://127.0.0.1:8000/python-environment/
DEBUG Received a cross-origin redirect. Removing sensitive headers.
DEBUG Received HTTP 302 Found. Redirecting to https://:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Handling request for https://:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/ with authentication policy auto
TRACE Request for https://:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://:eyJ2ZX.....zQ@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Attempting unauthenticated request for https://:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Request for https://:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://jfrog.acompany.com
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml.lock`
DEBUG Acquired exclusive lock for `credentials store`
DEBUG Released lock at `C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml.lock`
DEBUG No credentials file found at C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml
TRACE Considering retry of response HTTP 401 Unauthorized for https://:eyJ2ZX....zQ@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Cannot retry nested reqwest error
DEBUG Released lock at `C:\Users\mhuisman\AppData\Local\Temp\.tmpxeC3q4\simple-v18\index\12a4f10a4271579f\python-environment.lock`
DEBUG Indexes search failed because of status code failure: 401 Unauthorized
TRACE Received package metadata for: python-environment
DEBUG Searching for a compatible version of python-environment (*)
TRACE Selecting candidate for python-environment with range * with 0 remote versions
DEBUG No compatible version found for: python-environment
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on python-environment*
  python-environment not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on python-environment*
  python-environment not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because python-environment was not found in the package registry and you require python-environment, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (http://127.0.0.1:8000/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

**It also exposes the sensitive token in the logs (eyJ2ZX....zQ above)**
The url does work if I call it using requests or in browser with the empty username.

If i update my redirect test to include a username before the : it works
```
DEBUG No cache entry for: http://127.0.0.1:8000/python-environment/
TRACE Sending fresh GET request for http://127.0.0.1:8000/python-environment/
TRACE Handling request for http://127.0.0.1:8000/python-environment/ with authentication policy auto
TRACE Request for http://127.0.0.1:8000/python-environment/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://127.0.0.1:8000/python-environment/
TRACE Attempting unauthenticated request for http://127.0.0.1:8000/python-environment/
DEBUG Received a cross-origin redirect. Removing sensitive headers.
DEBUG Received HTTP 302 Found. Redirecting to https://jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Handling request for https://mhuisman%40acompany.com:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/ with authentication policy auto
TRACE Request for https://mhuisman%40acompany.com:****@jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/ already contains username and password
TRACE Considering retry of response HTTP 404 Not Found for https://jfrog.acompany.com/artifactory/api/pypi/matt-test/simple/python-environment/
TRACE Cannot retry nested reqwest error
DEBUG Released lock at `C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\index\12a4f10a4271579f\python-environment.lock`
TRACE Fetching metadata for python-environment from https://pypi.org/simple/python-environment/
TRACE Checking lock for `C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\pypi\python-environment.lock` at `C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\pypi\python-environment.lock`
DEBUG Acquired exclusive lock for `C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\pypi\python-environment.lock`
TRACE No cache entry exists for C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\pypi\python-environment.rkyv
DEBUG No cache entry for: https://pypi.org/simple/python-environment/
TRACE Sending fresh GET request for https://pypi.org/simple/python-environment/
TRACE Handling request for https://pypi.org/simple/python-environment/ with authentication policy auto
TRACE Request for https://pypi.org/simple/python-environment/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/python-environment/
TRACE Attempting unauthenticated request for https://pypi.org/simple/python-environment/
TRACE Request for https://pypi.org/simple/python-environment/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for URL https://pypi.org/simple
TRACE No credentials in cache for realm https://pypi.org
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml.lock`
DEBUG Acquired exclusive lock for `credentials store`
DEBUG Released lock at `C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml.lock`
DEBUG No credentials file found at C:\Users\mhuisman\AppData\Roaming\uv\credentials\credentials.toml
TRACE Considering retry of response HTTP 404 Not Found for https://pypi.org/simple/python-environment/
TRACE Cannot retry nested reqwest error
DEBUG Released lock at `C:\Users\mhuisman\AppData\Local\Temp\.tmpebVObi\simple-v18\pypi\python-environment.lock`
TRACE Received package metadata for: python-environment
DEBUG Searching for a compatible version of python-environment (*)
TRACE Selecting candidate for python-environment with range * with 0 remote versions
DEBUG No compatible version found for: python-environment
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on python-environment*
  python-environment not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on python-environment*
  python-environment not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because python-environment was not found in the package registry and you require python-environment, we can conclude that your requirements are unsatisfiable.
```

### Platform

Windows 11 64 bit

### Version

uv 0.9.22 (82a6a66b8 2026-01-06)

### Python version

3.10.9

---

_Label `bug` added by @matthuisman on 2026-01-07 04:29_

---

_Comment by @konstin on 2026-01-07 08:41_

CC @woodruffw 

---

_Comment by @woodruffw on 2026-01-07 12:59_

Thanks for the ping, I'll look at that credential redaction issue.

For that URL itself: RFC 3986's grammar doesn't seem to allow a leading colon like that, so I suspect that URL form is invalid and uv should probably reject it outright. However, I'd need to check what the WHATWG spec says as well.

Ref: https://www.rfc-editor.org/rfc/rfc3986#section-3.2.1

Edit: sorry, I misread, the RFC says alternation not concatenation. So that is indeed a valid URL.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-07 15:19_

---

_Comment by @woodruffw on 2026-01-07 15:21_

Noting for myself: the lack of redaction is because we send a `Url` for display, not a `DisplaySafeUrl`.

---

_Comment by @zanieb on 2026-01-07 15:34_

We won't forward credentials across realms, so if you redirect from one domain to another, the credentials are not propagated.

---

_Comment by @zanieb on 2026-01-07 15:37_

Oh, I see you're embedding the credentials in the proxy itself, nevermind my comment.

---

_Comment by @woodruffw on 2026-01-07 15:43_

#17346 will fix the redaction.

---

_Comment by @matthuisman on 2026-01-07 21:18_

its not a huge deal for us to use the username (email), but would prefer not too as it means we have to extract it from the jwt token in case it changes.

I was pretty much just trying to replicate your docs here: https://docs.astral.sh/uv/guides/integration/alternative-indexes/#authenticate-with-jwt-token but in the URL itself as jfrog doesn't like `__token__ `

---
