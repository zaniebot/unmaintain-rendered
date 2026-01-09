---
number: 2899
title: "failing to install a package from private pypi repo - Caused by: relative URL without a base"
type: issue
state: closed
author: etele
labels:
  - bug
assignees: []
created_at: 2024-04-08T10:24:18Z
updated_at: 2024-04-08T21:04:28Z
url: https://github.com/astral-sh/uv/issues/2899
synced_at: 2026-01-07T13:12:17-06:00
---

# failing to install a package from private pypi repo - Caused by: relative URL without a base

---

_Issue opened by @etele on 2024-04-08 10:24_

Failing to install a packge, from private pypi repo, the repo is set through `UV_INDEX_URL` environment variable.
Seems it finds the candidate to isntall, but in the end failing with 
```
error: Failed to download: pytz==2022.7.1
  Caused by: relative URL without a base
```
it happens for all of the packages, this is just one for demonstration.



Details and example:

platform:
```
$ uname -a
Linux 6.5.0-26-generic #26-Ubuntu SMP PREEMPT_DYNAMIC Tue Mar  5 21:19:28 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

uv version:
```
$ uv --version
uv 0.1.29
```

private repo settting:
```
$ env | grep UV_
UV_INDEX_URL=https://pypi.private.repo/simple/
```

the command:
```
$ uv -v pip install pytz==2022.7.1
INFO Found a virtualenv through VIRTUAL_ENV at: /home/user/.local/share/virtualenvs/api-v3W8N2Or
DEBUG Cached interpreter info for Python 3.11.8, skipping probing: /home/user/.local/share/virtualenvs/api-v3W8N2Or/bin/python
DEBUG Using Python 3.11.8 environment at /home/user/.local/share/virtualenvs/api-v3W8N2Or/bin/python
DEBUG Trying to lock if free: /home/user/.local/share/virtualenvs/api-v3W8N2Or/.lock
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: pytz==2022.7.1
TRACE Fetching metadata for pytz from https://pypi.private.repo/simple/pytz/
TRACE cached request https://pypi.private.repo/simple/pytz/ is storable because its response has a heuristically cacheable status code 200
TRACE cached request https://pypi.private.repo/simple/pytz/ has a cached response that does not allow staleness
TRACE request https://pypi.private.repo/simple/pytz/ does not have a fresh cache because its age is 1554 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.private.repo/simple/pytz/
DEBUG Sending revalidation request for: https://pypi.private.repo/simple/pytz/
DEBUG No credentials found for: https://pypi.private.repo/simple/pytz/
TRACE is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.private.repo/simple/pytz/
TRACE cached request https://pypi.private.repo/simple/pytz/ is storable because its response has a heuristically cacheable status code 200
TRACE Received package metadata for: pytz
TRACE selecting candidate for package PackageName("pytz") with range Range { segments: [(Included("2022.7.1"), Included("2022.7.1"))] } with 70 remote versions
TRACE found candidate for package PackageName("pytz") with range Range { segments: [(Included("2022.7.1"), Included("2022.7.1"))] } after 6 steps: "2022.7.1" version
error: Failed to download: pytz==2022.7.1
  Caused by: relative URL without a base
```

Same error when running it in a pipeline in a docker image, where the index is set through the requirements file:
```
$ cat requirements.txt 
-i https://pypi.private.rep/simple/
...
pytz==2022.7.1
...
```

---

_Comment by @charliermarsh on 2024-04-08 13:34_

Can you share what the download URLs look like in https://pypi.private.repo/simple/pytz/ ?

---

_Label `needs-mre` added by @charliermarsh on 2024-04-08 13:34_

---

_Comment by @etele on 2024-04-08 14:30_

Sure, here is a samle from it:

```html
<!DOCTYPE html>
<html>
<head>
    <meta name="pypi:repository-version" content="1.1">
    <title>Links for pytz</title>
</head>
<body>
    <h1>Links for pytz</h1>
    
      
        
          <a href="/packages/5a/d8/4d75d1e4287ad9d051aab793c68f902c9c55c4397636b5ee540ebd15aedf/pytz-2005k.tar.bz2?hash=597b596dc1c2c130cd0a57a043459c3bd6477c640c07ac34ca3ce8eed7e6f30c&remote=https://files.pythonhosted.org/packages/5a/d8/4d75d1e4287ad9d051aab793c68f902c9c55c4397636b5ee540ebd15aedf/pytz-2005k.tar.bz2#sha256=597b596dc1c2c130cd0a57a043459c3bd6477c640c07ac34ca3ce8eed7e6f30c" >pytz-2005k.tar.bz2</a>
        
      
    </br>
    
      
        
          <a href="/packages/73/b2/12575d53aa86b02dfb6d8ac808e2b0a866afaa598139aac4423795d1f150/pytz-2005k.tar.gz?hash=6b026144c04d29dc2f57e37754f9c62e99b2b9d8a863b2faf741af41956b3dcb&remote=https://files.pythonhosted.org/packages/73/b2/12575d53aa86b02dfb6d8ac808e2b0a866afaa598139aac4423795d1f150/pytz-2005k.tar.gz#sha256=6b026144c04d29dc2f57e37754f9c62e99b2b9d8a863b2faf741af41956b3dcb" >pytz-2005k.tar.gz</a>
        
      
    </br>
    ...
    
</body>
</html>
<!--SERIAL 21700107



-->
```

---

_Comment by @charliermarsh on 2024-04-08 14:42_

I assume that omitting the trailing slash from your index URL has no effect?

---

_Comment by @etele on 2024-04-08 14:54_

> I assume that omitting the trailing slash from your index URL has no effect?

no effect

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-08 16:56_

---

_Label `needs-mre` removed by @charliermarsh on 2024-04-08 16:56_

---

_Label `bug` added by @charliermarsh on 2024-04-08 16:56_

---

_Comment by @charliermarsh on 2024-04-08 16:56_

Ah yes, I see the issue.

---

_Comment by @charliermarsh on 2024-04-08 16:56_

We're getting confused by the `remote=https://...` piece. Will fix.

---

_Referenced in [astral-sh/uv#2904](../../astral-sh/uv/pulls/2904.md) on 2024-04-08 17:27_

---

_Closed by @charliermarsh on 2024-04-08 21:04_

---

_Referenced in [astral-sh/uv#2958](../../astral-sh/uv/issues/2958.md) on 2024-04-10 10:47_

---
