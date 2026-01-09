---
number: 8656
title: UV and dist-packages problem.
type: issue
state: closed
author: koodzi
labels:
  - question
assignees: []
created_at: 2024-10-29T11:28:47Z
updated_at: 2024-10-29T13:44:25Z
url: https://github.com/astral-sh/uv/issues/8656
synced_at: 2026-01-07T13:12:18-06:00
---

# UV and dist-packages problem.

---

_Issue opened by @koodzi on 2024-10-29 11:28_

I got a problem with `uv` not seeing system packages.

```Dockerfile
FROM debian

RUN apt update && apt install -yq python3 curl python3-requests
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/bin/uv
RUN uv pip install --system requests
```

Fails with:
```
0.371 Using Python 3.11.2 environment at /usr                                                                                                                  
0.372 error: The interpreter at /usr is externally managed, and indicates the following:      
```

When I add `--prefix /usr` or `/usr/local` uv installs the package, but it seems `uv` does not see packages in `dist-packages`

```Dockerfile
FROM debian

RUN apt update && apt install -yq python3 curl python3-requests
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/bin/uv
RUN python3 -c "import requests; print('>>>>', requests.__version__)"
RUN uv pip install --system --prefix /usr requests
RUN python3 -c "import requests; print('>>>>', requests.__version__)"


RUN python3 -c "import sys; print('>>>>', sys.prefix, sys.path)"
```
Output:
```
#9 [stage-0 4/7] RUN python3 -c "import requests; print('>>>>', requests.__version__)"
#9 0.343 >>>> 2.28.1
#9 DONE 0.4s

#10 [stage-0 5/7] RUN uv pip install --system --prefix /usr requests
#10 0.378 Using Python 3.11.2 environment at /usr
#10 6.522 Resolved 5 packages in 6.14s
#10 6.580 Prepared 5 packages in 56ms
#10 6.584 Installed 5 packages in 3ms
#10 6.584  + certifi==2024.8.30
#10 6.584  + charset-normalizer==3.4.0
#10 6.584  + idna==3.10
#10 6.584  + requests==2.32.3
#10 6.584  + urllib3==2.2.3
#10 DONE 6.7s

#11 [stage-0 6/7] RUN python3 -c "import requests; print('>>>>', requests.__version__)"
#11 0.330 >>>> 2.28.1
#11 DONE 0.4s

#12 [stage-0 7/7] RUN python3 -c "import sys; print('>>>>', sys.prefix, sys.path)"
#12 0.327 >>>> /usr ['', '/usr/lib/python311.zip', '/usr/lib/python3.11', '/usr/lib/python3.11/lib-dynload', '/usr/local/lib/python3.11/dist-packages', '/usr/lib/python3/dist-packages']
#12 DONE 0.4s
```

Do I think correctly that uv should see the requests in the system and use it instead of installing a new one?

Is there workaround ? Or maybe I miss some package?





---

_Comment by @charliermarsh on 2024-10-29 12:40_

You may want `--break-system-packages` if you're actually looking to modify an externally-managed Python.

---

_Label `question` added by @charliermarsh on 2024-10-29 12:40_

---

_Comment by @koodzi on 2024-10-29 13:44_

That's solved the problem.  Thank you.

---

_Closed by @koodzi on 2024-10-29 13:44_

---
