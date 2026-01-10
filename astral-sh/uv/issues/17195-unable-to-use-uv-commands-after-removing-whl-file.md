---
number: 17195
title: Unable to use uv commands after removing .whl file
type: issue
state: closed
author: jprince14
labels:
  - question
assignees: []
created_at: 2025-12-20T00:38:50Z
updated_at: 2025-12-20T01:30:09Z
url: https://github.com/astral-sh/uv/issues/17195
synced_at: 2026-01-10T01:26:14Z
---

# Unable to use uv commands after removing .whl file

---

_Issue opened by @jprince14 on 2025-12-20 00:38_

### Summary

I am using `uv` to install a locally build wheel within a docker container but things aren't working for me after I remove the .whl file during the docker build. I know I can leave the wheel in the container to get around this but I don't think that should be needed. Another thing that works is just activating the virtualenv with `.venv/bin/activate`

To reproduce place `dockerfile` and `test.sh` in the same directory and run `test.sh`

dockerfile
```dockerfile
FROM astral/uv:python3.11-trixie

RUN mkdir -p /home/testing
WORKDIR /home/testing
RUN uv init
RUN uv add paramiko
RUN wget https://files.pythonhosted.org/packages/c2/f8/49697181b1651d8347d24c095ce46c7346c37335ddc7d255833e7cde674d/ipaddress-1.0.23-py2.py3-none-any.whl
RUN uv add ipaddress-1.0.23-py2.py3-none-any.whl
RUN uv sync
RUN rm ipaddress-1.0.23-py2.py3-none-any.whl
ENTRYPOINT ["bash"]

CMD ["-c", "uv sync"]
# CMD ["-c", "uv run python --version"]

# This line below works with no issues
# CMD ["-c", "source .venv/bin/activate && python --version"]
```

test script (test.sh)
```bash
#!/bin/bash

docker build -t testing:latest .
docker run -it testing
```

Error that I see
```
error: Failed to generate package metadata for `ipaddress==1.0.23 @ path+ipaddress-1.0.23-py2.py3-none-any.whl`
  Caused by: Failed to read from the distribution cache
  Caused by: failed to query metadata of file `/home/testing/ipaddress-1.0.23-py2.py3-none-any.whl`: No such file or directory (os error 2)
```



### Platform

debian trixie

### Version

0.9.18

### Python version

3.11

---

_Label `bug` added by @jprince14 on 2025-12-20 00:38_

---

_Comment by @zanieb on 2025-12-20 01:01_

When you do `RUN uv add ipaddress-1.0.23-py2.py3-none-any.whl` you're adding a dependency on that file. When we `sync` again we need to validate that the file has not changed. I don't think this is a bug.

---

_Label `bug` removed by @zanieb on 2025-12-20 01:01_

---

_Label `question` added by @zanieb on 2025-12-20 01:01_

---

_Comment by @zanieb on 2025-12-20 01:02_

I'd recommend using `uv run --no-sync` (or `ENV UV_NO_SYNC=1`) instead of `uv run` if you are okay with the environment being stale.

---

_Comment by @jprince14 on 2025-12-20 01:30_

Thats easy, thanks for the help!

---

_Closed by @jprince14 on 2025-12-20 01:30_

---
