```yaml
number: 13756
title: "`uv run python3` handles SIGINT incorrectly"
type: issue
state: closed
author: nemowang2003
labels:
  - bug
assignees: []
created_at: 2025-05-31T21:33:43Z
updated_at: 2025-06-01T15:10:55Z
url: https://github.com/astral-sh/uv/issues/13756
synced_at: 2026-01-12T16:01:36Z
```

# `uv run python3` handles SIGINT incorrectly

---

_@nemowang2003_

### Summary

`uv run python3 demo.py` acts differently with directly running `python3 demo.py` with programs handling SIGINT(aka. `KeyboardInterrupt`).

## Reproduction

```python
# demo.py
import time


def wait():
    try:
        time.sleep(5)
    except KeyboardInterrupt:
        print("inside")


while True:
    try:
        input()
        wait()
    except KeyboardInterrupt:
        print("outside")
    except EOFError:
        break
```

The action is：
1. hit enter(to enter nothing)
2. hit Control+C to send SIGINT(in 5 seconds, to interrupt `wait` function)
3. hit enter(to enter nothing again)
4. hit control+C(again, to interrup `wait` function)

In normal `python3 demo.py`, the result is:
```text

^Cinside

^Cinside
```

In `uv run -v python3 demo.py`, the result (with log) is:
```text
DEBUG Running `python3 demo.py`
DEBUG Spawned child 91884 in process group 91883

^Cinside
DEBUG Received SIGINT, assuming the child received it as part of the process group

^CDEBUG Received SIGINT, forwarding to child at 91884 in 200ms
inside
outside
```

### Platform

Darwin 22.6.0 arm64

### Version

uv 0.7.8 (Homebrew 2025-05-23)

### Python version

Python 3.13.3

---

_Label `bug` added by @nemowang2003 on 2025-05-31 21:33_

---

_Comment by @nemowang2003 on 2025-05-31 21:41_

And when I deleted the `input()` line, the behavior changed again.
```python
# demo.py
import time


def wait():
    try:
        time.sleep(5)
    except KeyboardInterrupt:
        print("inside")


while True:
    try:
        wait()
    except KeyboardInterrupt:
        print("outside")

```
Now when I repeat the step2 & step4 mentioned above, `python3 demo.py` behaves the same.
However `uv run -v python3 demo.py` outputs this:
```text
DEBUG Running `python3 demo.py`
DEBUG Spawned child 92156 in process group 92155
^Cinside
DEBUG Received SIGINT, assuming the child received it as part of the process group
^Cinside
DEBUG Received SIGINT, forwarding to child at 92156 in 200ms
inside
```


---

_Comment by @charliermarsh on 2025-05-31 23:28_

Have you read the other open issues about SIGINT?

---

_Comment by @zanieb on 2025-06-01 04:58_

i.e., https://github.com/astral-sh/uv/issues/12108

---

_Comment by @nemowang2003 on 2025-06-01 05:55_

I see why it's designed to forward SIGINT when received at second time.
I am working on my personal scripts, therefore it's really not quite a common usage.
Thank you for response, I think I should close this issue as duplicated.

---

_Closed by @nemowang2003 on 2025-06-01 05:55_

---

_Comment by @zanieb on 2025-06-01 15:10_

Thanks!

---
