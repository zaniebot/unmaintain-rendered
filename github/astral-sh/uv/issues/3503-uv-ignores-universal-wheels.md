---
number: 3503
title: uv ignores universal wheels
type: issue
state: closed
author: RonnyPfannschmidt
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-05-10T09:19:46Z
updated_at: 2024-05-10T13:20:50Z
url: https://github.com/astral-sh/uv/issues/3503
synced_at: 2026-01-07T13:12:17-06:00
---

# uv ignores universal wheels

---

_Issue opened by @RonnyPfannschmidt on 2024-05-10 09:19_

while working on a pinning script for internal packages,
i observed uv ignore any package using a universal wheel instead of a python3 only wheel

this happend because the afflicted packages where using hach without declaring requires_python
in turn hatch created a universal wheel ( `internal_redacted_plugin-24.5.9.0-py2.py3-none-any.whl`)

i believe uv should consider universal wheel (and warn about them, as these days seeing them indicates a packaging mistake)

---

_Comment by @RonnyPfannschmidt on 2024-05-10 09:26_

i just took note that this is expedited by any codebase that still is depending on the cached-property package 

---

_Comment by @RonnyPfannschmidt on 2024-05-10 11:10_

``` 
$  uv pip install <redacted>==2024.5.10.0 --index-url=<redacted> --native-tls --no-deps -v
INFO Found a virtualenv through VIRTUAL_ENV at: <redacted>
DEBUG Cached interpreter info for Python 3.9.19, skipping probing: <redacted>
DEBUG Using Python 3.9.19 environment at <redacted>
DEBUG Trying to lock if free: <redacted>
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.9.19
DEBUG Adding direct dependency: <redacted>==2024.5.10.0
DEBUG Found fresh response for: https://repository.<redacted>/simple/<redacted>
DEBUG Searching for a compatible version of <redacted> (==2024.5.10.0)
DEBUG No compatible version found for: <redacted>
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of <redacted>==2024.5.10.0 and you require <redacted>==2024.5.10.0, we can conclude that the requirements are unsatisfiable.
```

vs 
```
$  pip install <redacted>==2024.5.10.0 --index-url='https://<redacted>/nexus/repository/<redacted>/simple' --no-deps -v
Using pip 24.0 from <redacted>site-packages/pip (python 3.9)
Looking in indexes: https://<redacted>/nexus/repository/<redacted>/simple
Collecting <redacted>==2024.5.10.0
  Downloading https://<redacted>/nexus/repository/<redacted>/packages/<redacted>/2024.5.10.0/<redacted>-2024.5.10.0-py3-none-any.whl (346 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 346.9/346.9 kB 400.9 kB/s eta 0:00:00
Installing collected packages: <redacted>
Successfully installed <redacted>-2024.5.10.0
```

it seems i just observed the same using a non-universal wheel

---

_Comment by @RonnyPfannschmidt on 2024-05-10 11:13_

i need to reevaluate the actual reason - in initial testing i did observe a error for cached_property among other dependencies, however currently when trying in isolation it seems to work fine


---

_Label `compatibility` added by @konstin on 2024-05-10 11:16_

---

_Label `bug` added by @konstin on 2024-05-10 11:16_

---

_Comment by @RonnyPfannschmidt on 2024-05-10 11:17_

i can no longer observe the issue after clearing the uv cache - i suspect i suffered a corrupt cache, its not clear how i got into that but once i started rerunning again, it started to work again


---

_Comment by @RonnyPfannschmidt on 2024-05-10 11:22_

before the behaviour started, i ran a `uv pip compile` across a matrix of several python platforms and python version

---

_Comment by @charliermarsh on 2024-05-10 13:16_

It may be that you needed to run with `--refresh` to force a refresh of the simple API response. We respect the caching headers (10 minutes).

---

_Comment by @charliermarsh on 2024-05-10 13:20_

I'm going to close as I don't know if there's anything actionable here now, but happy to revisit if it comes up again.

---

_Closed by @charliermarsh on 2024-05-10 13:20_

---
