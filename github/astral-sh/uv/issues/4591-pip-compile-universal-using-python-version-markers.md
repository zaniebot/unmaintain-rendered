---
number: 4591
title: pip compile --universal using python version markers
type: issue
state: closed
author: bluss
labels:
  - bug
  - lock
assignees: []
created_at: 2024-06-27T17:49:53Z
updated_at: 2024-06-27T18:41:46Z
url: https://github.com/astral-sh/uv/issues/4591
synced_at: 2026-01-07T13:12:17-06:00
---

# pip compile --universal using python version markers

---

_Issue opened by @bluss on 2024-06-27 17:49_

Python version markers being used in an interesting way by pip compile --universal.

Bug summary: the emitted markers `python_version >= '3.12'` etc make the lockfile non-universal. Without -p argument the result depends on which python version uv selects by default.


uv 0.2.17
platform: linux (x86_64)

```shell
> echo pandas==2.2.2 | uv pip compile --universal - -p 3.12 --no-header
Resolved 6 packages in 18ms
numpy==2.0.0 ; python_version >= '3.12'
    # via pandas
pandas==2.2.2
python-dateutil==2.9.0.post0
    # via pandas
pytz==2024.1
    # via pandas
six==1.16.0
    # via python-dateutil
tzdata==2024.1
    # via pandas
```

```shell
> echo pandas==2.2.2 | uv pip compile --universal - -p 3.11 --no-header
Resolved 6 packages in 18ms
numpy==2.0.0 ; python_version == '3.11'
    # via pandas
pandas==2.2.2
python-dateutil==2.9.0.post0
    # via pandas
pytz==2024.1
    # via pandas
six==1.16.0
    # via python-dateutil
tzdata==2024.1
    # via pandas
```

```shell
> echo pandas==2.2.2 | uv pip compile --universal - -p 3.10 --no-header
Resolved 6 packages in 18ms
numpy==2.0.0 ; python_version < '3.11'
    # via pandas
pandas==2.2.2
python-dateutil==2.9.0.post0
    # via pandas
pytz==2024.1
    # via pandas
six==1.16.0
    # via python-dateutil
tzdata==2024.1
    # via pandas
```

---

_Comment by @charliermarsh on 2024-06-27 17:51_

We need some way to understand the minimum Python version compatibility -- I don't think this is a "bug"?

---

_Label `lock` added by @zanieb on 2024-06-27 17:54_

---

_Comment by @bluss on 2024-06-27 17:54_

It looks like a bug because the resolution done for python 3.10 is only valid for python 3.10 (not for higher versions). (The `numpy` dependency is not pinned for python 3.11, 3.12 - this "works" but there is no lock for that dependency?)

```
      --universal
          Perform a universal resolution, attempting to generate a single `requirements.txt` output
          file that is compatible with all operating systems, architectures and supported Python
          versions
```

The docs suggest that universal can create a lockfile that is valid for more than one python version I think?

---

_Label `question` added by @zanieb on 2024-06-27 17:55_

---

_Comment by @charliermarsh on 2024-06-27 17:55_

Yes, the Python version is meant to be a minimum. Just to clarify, there's no such thing as locking for "all" Python versions due to `requires-python`. You would need _all_ of your dependencies to support all Python versions in order to produce a resolution -- but that's never true. So we need input on the minimum bound.

That being said, the marker output here is clearly a bit weird? Like `numpy==2.0.0 ; python_version < '3.11'` is clearly wrong for `-p 3.10` -- so I take it back. The intent is that `-p 3.10` locks for Python >= 3.10, so that's a bug.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-27 17:55_

---

_Label `question` removed by @charliermarsh on 2024-06-27 17:55_

---

_Label `bug` added by @charliermarsh on 2024-06-27 17:55_

---

_Comment by @zanieb on 2024-06-27 17:56_

@charliermarsh I thought you you intentionally had it pin for the current version instead of using it as a lower bound?

---

_Comment by @charliermarsh on 2024-06-27 17:58_

The _intent_ was for it to be a minimum version.

---

_Referenced in [astral-sh/uv#4593](../../astral-sh/uv/issues/4593.md) on 2024-06-27 18:08_

---

_Comment by @charliermarsh on 2024-06-27 18:25_

Ok this is a one-line change but we need to decide what we want: minimum version or exact version?

---

_Referenced in [astral-sh/uv#4597](../../astral-sh/uv/pulls/4597.md) on 2024-06-27 18:29_

---

_Closed by @charliermarsh on 2024-06-27 18:41_

---
