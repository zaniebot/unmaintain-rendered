```yaml
number: 11214
title: "v0.5.27 changes behavior for `uv pip sync -r requirements.txt` (breaking)"
type: issue
state: closed
author: mzoll
labels:
  - bug
assignees: []
created_at: 2025-02-04T12:11:07Z
updated_at: 2025-02-04T21:41:39Z
url: https://github.com/astral-sh/uv/issues/11214
synced_at: 2026-01-10T03:50:31Z
```

# v0.5.27 changes behavior for `uv pip sync -r requirements.txt` (breaking)

---

_Issue opened by @mzoll on 2025-02-04 12:11_

### Summary

Issue Description
--------------------
0.5.27 of uv  changed the behavior for by hand creating an virtual environment and trying to install packages into it. 

Error is:
'''
##[error] > [builder  7/15] RUN uv pip sync --no-cache-dir -r requirements.txt:
##[error]0.312 error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
'''

Running the example code with uv==0.5.26 works fine.

The Changelog for 0.5.27 does not list any differences which could explain this error.


Steps to reproduce 
---------------------
(Docker):
```
ARG PYTHON_VERSION=3.10

# === builder stage ===
FROM python:${PYTHON_VERSION}-slim AS builder

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /detectors

RUN pip install uv==0.5.27

RUN uv --version
RUN uv venv /opt/venv --python ${PYTHON_VERSION} --no-project

ENV PATH="/opt/venv/bin:$PATH"

# install requirements.txt
COPY requirements.txt .
RUN uv pip sync --no-cache-dir -r requirements.txt # <<< Error
```


### Platform

docker/Linux

### Version

0.5.27

### Python version

3.10

---

_Label `bug` added by @mzoll on 2025-02-04 12:11_

---

_Comment by @mzoll on 2025-02-04 12:14_

Apologies that i have not fine grained tested yet, where this errors seem to originate from, resp make this into a more minimalistic example

---

_Comment by @mzoll on 2025-02-04 12:22_

One method of solving this, was by running a 'RUN source venv/bin/activate' within the docker, which however is not working as `source` is not preinstalled in this minimalistic docker-img.

---

_Renamed from "v0.5.27 changes behavior for `uv pip sync -r requirements.txt`" to "v0.5.27 changes behavior for `uv pip sync -r requirements.txt` (breaking)" by @mzoll on 2025-02-04 12:22_

---

_Comment by @spatel033 on 2025-02-04 13:18_

Temporarily fixed by `pip install uv==0.5.26`

---

_Comment by @tuxu on 2025-02-04 13:22_

I also just stumbled over this. You can set `VIRTUAL_ENV` as a workaround:

```dockerfile
ENV VIRTUAL_ENV="/opt/venv"
```

However, I can't imagine that this was intentional.


---

_Assigned to @zanieb by @zanieb on 2025-02-04 15:13_

---

_Comment by @zanieb on 2025-02-04 15:22_

Hi!

I'm sorry this broke your code. I'm surprised to see this pattern in the wild — this was an intentional choice in #11143 which solves other problems.

https://github.com/astral-sh/uv/blob/e6ead20aac1efcb41a8826ef409109f6f17189ca/crates/uv-python/src/discovery.rs#L1628-L1632

I will see if I can find a reasonable fix.

---

_Comment by @zanieb on 2025-02-04 15:23_

I still sort of feel my original sentiment in that comment. Maybe if it's at the front of the path though?

---

_Comment by @zanieb on 2025-02-04 19:56_

I'm pretty on the fence about special casing this as in https://github.com/astral-sh/uv/pull/11218

---

_Closed by @zanieb on 2025-02-04 21:41_

---
