---
number: 2964
title: Running uv for the second time is slower on a mounted docker volume
type: issue
state: closed
author: jaap3
labels:
  - performance
assignees: []
created_at: 2024-04-10T14:25:17Z
updated_at: 2024-04-16T08:15:08Z
url: https://github.com/astral-sh/uv/issues/2964
synced_at: 2026-01-07T13:12:17-06:00
---

# Running uv for the second time is slower on a mounted docker volume

---

_Issue opened by @jaap3 on 2024-04-10 14:25_

I'm looking into replacing `pip` with `uv` in our development setup. We develop our projects in `docker` containers and one thing we do is (re)install all requirements when the container is (re)started. I noticed that in some cases `pip` was faster than `uv`, so I did some testing.

I created a test `requirements.txt` file:
```requirements.txt
Django~=4.2.0
django-email-bandit~=2.0.0
djangorestframework~=3.14.0
django-filter~=23.2
leukeleu-django-checks~=1.2
leukeleu-django-gdpr~=1.4
psycopg2-binary~=2.9.6
sentry-sdk~=1.18
wagtail~=5.1.0
coverage[toml]~=7.2
pywatchman~=2.0.0
ruff~=0.3.2
```

A minimal `pip.sh`:
```bash
#!/usr/bin/env bash
python -m venv ./venv/pip
source ./venv/pip/bin/activate
pip install -r requirements.txt
```

and a minimal `uv.sh`:
```bash
#!/usr/bin/env bash
uv venv ./venv/uv
source ./venv/uv/bin/activate
uv pip install -r requirements.txt
```

I then started a `python` container using 

```
docker run --rm -itv `pwd`:/code -w /code python bash
```

This container runs Python 3.12.3 with pip 24.0.

I used `pip` to install `uv` at version 0.1.31

I then timed both scripts multiple times, to get the times for a cold (no venv/cache) and warm (existing venv/cach) install:

```shell
# time ./pip.sh
...
real	0m25.336s
user	0m9.570s
sys	0m2.723s

# time ./pip.sh
...
real	0m2.013s
user	0m1.614s
sys	0m0.153s

# time ./uv.sh 
...
real	0m5.579s
user	0m2.018s
sys	0m2.426s

# time ./uv.sh
...
real	0m7.100s
user	0m0.233s
sys	0m1.833s
```

As expected, on a cold install `uv` outperforms `pip` by a wide margin. However, unexpectedly, the second install is slower (it's even slower than `uv` first run).

I suspected that the mounted volume had something to do with this. So I copied the scripts and requirements file to another directory in the container (one that is not mounted) and timed everything again (note that I did *not* remove the pip and uv download caches):

```shell
# time ./pip.sh
...
real	0m7.413s
user	0m6.079s
sys	0m0.491s

# time ./pip.sh
...
real	0m1.627s
user	0m1.543s
sys	0m0.085s

# time ./uv.sh 
...
real	0m0.174s
user	0m0.073s
sys	0m0.270s

# time ./uv.sh
...
real	0m0.232s
user	0m0.061s
sys	0m0.319s
```

This time `uv` beats `pip` easily (however the second invocation is still somewhat slower than the initial one).

I'm using Docker version 25.0.3 on MacOS Sonoma 14.4.1. VirtioFS is enabled for bind mounts.

---

_Label `performance` added by @zanieb on 2024-04-10 14:35_

---

_Comment by @notatallshaw on 2024-04-10 15:40_

Due to bigger build images it's generally considered best practice inside docker to set pip to use no cache e.g. via `--no-cache-dir`.

I imagine the same would be for true for uv, e.g. via `--no-cache`.

Is there a particular use case you have for installing Python packages twice in a docker image?

---

_Comment by @jaap3 on 2024-04-10 17:22_

@notatallshaw this is not about baking dependencies into an image. This is about using a fairly bare bones python image to mount a project and installing it's dependencies into a venv that's also in the mounted directory.

Re-installing happens when the container is restarted (or recreated) when switching branches, pulling work etc.

---

_Referenced in [astral-sh/uv#3015](../../astral-sh/uv/pulls/3015.md) on 2024-04-12 14:54_

---

_Comment by @konstin on 2024-04-12 15:13_

I can't reproduce this on ubuntu, so i'm speculating for macos.

```
root:/code# time ./pip.sh
real    0m17.502s
root:/code# time ./pip.sh
real    0m1.438s
root:/code# time ./uv.sh
real    0m5.285s
root:/code# time ./uv.sh
real    0m0.242s
root:/code# time ./uv.sh
real    0m0.339s
```

When uv installs a wheel, we first unpack the wheel to the cache, then use that cache to populate the venv. On macos, we clone the directory by default, on linux and windows, we hardlink by default; these are a lot faster (and space efficient) than copying the files. Should cloning/hardlinking fail, we fall back to copying all files. We were missing logging for the fallback switchover for hardlinking, i've added this in #3015, afterwards you should be able to see it with RUST_LOG=debug.

The cache by default is in `~/.cache/uv` inside the docker container, while the target venv is a bind mount, so hardlinking fails and we end up copying all files. It looks like this copying is slow with the macos docker fs driver, and slower than reading and unpacking a single zip archive that python does.

---

_Comment by @jaap3 on 2024-04-15 08:16_

@konstin Thanks for looking into this. I just timed everything again, but this time I used the `--cache-dir` flag for both `pip` and `uv` to put the cache on the mounted volume.

The time it takes for `pip` is pretty much the same.

```
# time ./pip.sh
real	0m24.987s

# time ./pip.sh
real	0m1.951s
```

`uv` becomes slower if the cache is also on the mounted volume:

```
# time ./uv.sh
real	0m12.715s

# time ./uv.sh
real	0m9.178s
```

Will try this again with `RUST_LOG=debug` when the next release is out.

---

_Comment by @jaap3 on 2024-04-16 08:15_

I just tried again with `uv 0.1.32` and `RUST_LOG=debug`.

I see a number of logs about failed hardlinks when the `cache-dir` is outside the mounted volume. I *don't* see these when the `cache-dir` is on the mounted volume, but that makes no real difference when it comes to install speed (as I noted yesterday).

I now also realise that the extra time it takes to do a second install is caused by `uv` removing the old `venv` before recreating it. Turns out `python -m venv ./var/venv` will just keep an existing intact, so `pip` had a major advantage there.

Conclusion: mounted volumes in Docker on macOS are slow and I shouldn't have assumed that `uv venv <path>` `python -m venv <path>` behaves in the same way.

I don't think there's anything `uv` can really do to optimise this. I did learn a bit more about how `uv` works, that I can use in our setup, so there's a silver lining 😀.

---

_Closed by @jaap3 on 2024-04-16 08:15_

---
