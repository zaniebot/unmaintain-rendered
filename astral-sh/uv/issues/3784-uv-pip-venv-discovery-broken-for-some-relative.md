```yaml
number: 3784
title: uv pip venv discovery broken for some relative paths
type: issue
state: closed
author: bulletmark
labels:
  - bug
assignees: []
created_at: 2024-05-23T05:04:08Z
updated_at: 2024-05-24T16:18:50Z
url: https://github.com/astral-sh/uv/issues/3784
synced_at: 2026-01-10T05:31:37Z
```

# uv pip venv discovery broken for some relative paths

---

_Issue opened by @bulletmark on 2024-05-23 05:04_

I am using:
```
$  uv --version
uv 0.2.2
```
I create a venv and the let `uv pip` discover that at the default `.venv/`:
```
$ uv venv
Using Python 3.12.3 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
lt:~ uv pip list -v
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Found a virtual environment at: /home/mark/.venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python
```

However, I tend to use `$VIRTUAL_ENV` to relatively specify my various venv paths and this is broken since 0.2.2 (probably since 0.2.0?):
```
# In same directory as above where  I just created `.venv`
$ export VIRTUAL_ENV=.venv
$ uv pip list -v
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Searching for interpreter that fulfills Python @ default
INFO Found active virtual environment (via VIRTUAL_ENV) at: .venv
DEBUG Found a virtual environment at: /home/mark/.venv
DEBUG Using Python 3.12.3 environment at Data/src/gh/dyndns/.venv/bin/python
Package        Version
-------------- -------
aiohttp        3.9.5
... etc
```

Note the "Found a .." which is correct and the next "Using .." which is bogus. I have no idea how it picked up that bogus venv.

If I set `export VIRTUAL_ENV=./.venv` or I set it to an absolute path it works.

---

_Comment by @bulletmark on 2024-05-23 05:09_

Actually I just noticed this is broken even if I specify the venv path, i.e. `uv pip list -p .venv` is broken,
but  `uv pip list -p ./.venv` and `uv pip list -p /fullpath/.venv` both work.

---

_Comment by @zanieb on 2024-05-23 12:46_

Hi @bulletmark thanks for the details.

Could you share logs with trace logs i.e. `RUST_LOG=uv=trace uv pip ...`?

---

_Label `bug` added by @zanieb on 2024-05-23 12:46_

---

_Assigned to @zanieb by @zanieb on 2024-05-23 12:46_

---

_Comment by @zanieb on 2024-05-23 12:54_

I can't reproduce this with a simple case

```
❯ uv pip install --python .venv anyio -v
DEBUG Searching for interpreter that fulfills directory .venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
...
Audited 1 package in 4ms
```

```
❯ VIRTUAL_ENV=.venv uv pip install anyio -v
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Searching for interpreter that fulfills Python @ default
INFO Found active virtual environment (via VIRTUAL_ENV) at: .venv
DEBUG Found a virtual environment at: /Users/zb/workspace/uv/.venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
...
Audited 1 package in 0.80ms
```

---

_Label `needs-mre` added by @zanieb on 2024-05-23 13:09_

---

_Comment by @bulletmark on 2024-05-23 22:45_

```
# Create a new directory for this test:
$ mkdir uv-issue-3784
$ cd uv-issue-3784
$ ls -la
total 16
drwxr-xr-x  2 mark mark  4096 May 24 08:33 .
drwx------ 99 mark mark 12288 May 24 08:33 ..

# Create a new simple venv at .venv:
$ uv venv
Using Python 3.12.3 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ ls -la
total 20
drwxr-xr-x  3 mark mark  4096 May 24 08:33 .
drwx------ 99 mark mark 12288 May 24 08:33 ..
drwxr-xr-x  4 mark mark  4096 May 24 08:33 .venv

# Normal uv pip list works fine:
$ uv pip list -v
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Searching for interpreter that fulfills Python @ default
DEBUG Found a virtual environment at: /home/mark/uv-issue-3784/.venv
DEBUG Using Python 3.12.3 environment at .venv/bin/python

# Specify .venv explicitly and it somehow chooses a bogus one:
$ uv pip list -v -p .venv
DEBUG Searching for interpreter that fulfills directory .venv
DEBUG Using Python 3.12.3 environment at /home/mark/Data/src/gh/dyndns/.venv/bin/python
Package        Version
-------------- -------
aiohttp        3.9.5
aiosignal      1.3.1
argcomplete    3.3.0
attrs          23.2.0
frozenlist     1.4.1
idna           3.7
inotify-simple 1.3.5
markdown-it-py 3.0.0
mdurl          0.1.2
multidict      6.0.5
platformdirs   4.2.2
pygments       2.18.0
rich           13.7.1
rich-argparse  1.4.0
yarl           1.9.4

# Do same as above and capture RUST_LOG TRACE as requested:
$ RUST_LOG=uv=trace uv pip list -v -p .venv
DEBUG Searching for interpreter that fulfills directory .venv
TRACE Cached interpreter info for Python 3.12.3, skipping probing: .venv/bin/python
DEBUG Using Python 3.12.3 environment at /home/mark/Data/src/gh/dyndns/.venv/bin/python
Package        Version
-------------- -------
aiohttp        3.9.5
aiosignal      1.3.1
argcomplete    3.3.0
attrs          23.2.0
frozenlist     1.4.1
idna           3.7
inotify-simple 1.3.5
markdown-it-py 3.0.0
mdurl          0.1.2
multidict      6.0.5
platformdirs   4.2.2
pygments       2.18.0
rich           13.7.1
rich-argparse  1.4.0
yarl           1.9.4

# Show with RUST_LOG that it works if we specify ./.venv instead of .venv:
$ RUST_LOG=uv=trace uv pip list -v -p ./.venv
DEBUG Searching for interpreter that fulfills directory ./.venv
TRACE Cached interpreter info for Python 3.12.3, skipping probing: ./.venv/bin/python
DEBUG Using Python 3.12.3 environment at /home/mark/.venv/bin/python
```

---

_Comment by @bulletmark on 2024-05-23 22:54_

Actually, just noticed that the last example above (`uv pip list -v -p ./.venv)` doesn't work either as it is selecting a different bogus venv  `~/.venv` which is an empty one I created for a previous demo for this bug. If I delete that `~/.venv` and then run that same test in the same dir above then I get:
```
$ RUST_LOG=uv=trace uv pip list -v -p ./.venv
DEBUG Searching for interpreter that fulfills directory ./.venv
TRACE Cached interpreter info for Python 3.12.3, skipping probing: ./.venv/bin/python
DEBUG Using Python 3.12.3 environment at /home/mark/.venv/bin/python
```

I.e. it still seems to want to use that bogus `/home/mark/.venv` even though it is now deleted? But there is no error reporting when listing it so I presume it somehow fell back to the correct `./.venv`?

---

_Comment by @bulletmark on 2024-05-23 23:02_

Seems to me that uv is caching the name of `venvs` that get created and that cache is based on the name passed on the command line (or environment var) even if it is a relative path but it must be cached based on absolute path name.

---

_Comment by @zanieb on 2024-05-23 23:26_

Thanks for the details! I'll dig into this

---

_Label `needs-mre` removed by @zanieb on 2024-05-23 23:26_

---

_Comment by @ArthurZucker on 2024-05-24 08:25_

Hard to reproduce, but I think our CIs are broken for something similar: https://app.circleci.com/pipelines/github/huggingface/transformers/93822/workflows/50779f61-f59f-483d-bc3f-9b173dd4cc15/jobs/1231217 starting uv==0.2.2

- we build a docker image with uv venv
- then we download the docker and we install stuff
- only uv is shown to be installed. 


---

_Comment by @ArthurZucker on 2024-05-24 08:25_

We also use VIRTUALENV: https://github.com/huggingface/transformers/blob/046c2ad7923bb0601f53d6f32c3092aa697ab14d/docker/quality.dockerfile#L1 

---

_Comment by @charliermarsh on 2024-05-24 13:35_

Ah yeah, we stopped supporting the use of `VIRTUAL_ENV` to point to non-virtual environments in v0.2. You could try either telling uv to discover the system Python:

```
ENV UV_SYSTEM=1
```

Or point it to a specific Python:

```
ENV UV_PYTHON=/usr/local/bin/python
```


---

_Comment by @zanieb on 2024-05-24 14:30_

~Hey @bulletmark, just want to check — are the listed packages correct regardless of the log message about which interpreter is being used??~ Nevermind

I reproduced this and have a fix up at https://github.com/astral-sh/uv/pull/3823

---

_Closed by @zanieb on 2024-05-24 16:18_

---
