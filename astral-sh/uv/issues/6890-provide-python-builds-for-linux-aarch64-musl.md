---
number: 6890
title: provide Python builds for linux-aarch64-musl
type: issue
state: closed
author: mikolysz
labels:
  - enhancement
assignees: []
created_at: 2024-08-30T23:30:48Z
updated_at: 2025-12-21T21:47:32Z
url: https://github.com/astral-sh/uv/issues/6890
synced_at: 2026-01-10T01:24:07Z
---

# provide Python builds for linux-aarch64-musl

---

_Issue opened by @mikolysz on 2024-08-30 23:30_

linux-aarch64-musl is the platform one gets when using an Alpine-based Docker image and developing on Apple Silicon, which is a pretty common setup at this point. This set up currently (as of uv 0.4.1) doesn't support installing Python via uv, with the following error:

```
Searching for Python installations
error: No download found for request: cpython-any-linux-aarch64-musl
```

For other users encountering this issue, installing Python via `app add python3` seems to work for now, although Alpine's packages tend to be fairly minimal, so more dependencies might be needed for larger projects.

---

_Label `enhancement` added by @charliermarsh on 2024-09-01 15:55_

---

_Comment by @charliermarsh on 2024-09-01 15:56_

This is known but can't find an existing issue to link to -- will leave it up for now.

---

_Comment by @charliermarsh on 2024-09-01 15:56_

Related to https://github.com/astral-sh/uv/pull/6643.

---

_Comment by @zanieb on 2024-09-03 14:20_

Yeah more context in

- https://github.com/astral-sh/uv/issues/4242
- https://github.com/astral-sh/uv/issues/6392

---

_Referenced in [astral-sh/uv#6392](../../astral-sh/uv/issues/6392.md) on 2024-09-03 14:21_

---

_Referenced in [astral-sh/uv#9428](../../astral-sh/uv/issues/9428.md) on 2024-11-25 21:39_

---

_Referenced in [astral-sh/uv#9430](../../astral-sh/uv/pulls/9430.md) on 2024-11-25 22:39_

---

_Comment by @SamuelMarks on 2025-01-01 16:48_

What's the current progress here? - Need a hand?

---

_Comment by @zanieb on 2025-01-01 17:37_

If you're interested in lending a hand it's definitely welcome! I haven't started working on this part of `python-build-standalone` yet.

Briefly, it looks like there were some previous discussions:

- https://github.com/astral-sh/python-build-standalone/issues/86
- https://github.com/astral-sh/python-build-standalone/issues/260
- https://github.com/astral-sh/python-build-standalone/issues/221

https://github.com/astral-sh/python-build-standalone/issues/86 seems like the best place to discuss implementation details, if necessary.

---

_Comment by @SamuelMarks on 2025-01-02 03:06_

Ok, just took a look: https://github.com/astral-sh/python-build-standalone/releases/tag/20241219 doesn't have a build for my architecture + Linux + musl.

I'll get my other laptop back in a couple of days will try then

Happy New Year!

---

_Referenced in [astral-sh/uv#7529](../../astral-sh/uv/issues/7529.md) on 2025-01-06 20:07_

---

_Referenced in [tonybaloney/CSnakes#324](../../tonybaloney/CSnakes/issues/324.md) on 2025-01-09 09:23_

---

_Comment by @mwarkentin on 2025-02-06 14:51_

I ran into this when attempting to use `uv` inside of an Alpine docker container (which is the first "derived image" mentioned on https://docs.astral.sh/uv/guides/integration/docker/#available-images. 

My use case: working on a docker container that contains a grab bag of scripts that our team has built over time, and hoping to use `uv` to manage python versions and dependencies which may be different for each script, using the inline `/// script` definitions.

Will switch over to a Debian image for now instead.

---

_Referenced in [astral-sh/uv#11307](../../astral-sh/uv/issues/11307.md) on 2025-02-07 08:15_

---

_Referenced in [astral-sh/setup-uv#278](../../astral-sh/setup-uv/issues/278.md) on 2025-02-13 19:50_

---

_Referenced in [rstudio/reticulate#1751](../../rstudio/reticulate/issues/1751.md) on 2025-02-27 13:41_

---

_Comment by @zanieb on 2025-03-12 02:09_

We support these as of 0.6.6 — https://github.com/astral-sh/uv/pull/12121 

---

_Closed by @zanieb on 2025-03-12 02:10_

---

_Referenced in [astral-sh/uv#12286](../../astral-sh/uv/issues/12286.md) on 2025-03-18 15:08_

---

_Comment by @zanieb on 2025-03-18 15:15_

Sorry about the confusion — there are `linux-x86_64-musl` builds but not aarch64 builds yet.

---

_Reopened by @zanieb on 2025-03-18 15:15_

---

_Comment by @mio-19 on 2025-04-06 23:53_

Would it be difficult to add aarch64-musl?

---

_Comment by @zanieb on 2025-04-07 18:11_

Yeah, it is. See https://github.com/astral-sh/python-build-standalone/pull/569#issue-2933229417

---

_Referenced in [danny-avila/LibreChat#7063](../../danny-avila/LibreChat/issues/7063.md) on 2025-05-01 23:35_

---

_Referenced in [danny-avila/LibreChat#7270](../../danny-avila/LibreChat/pulls/7270.md) on 2025-05-07 20:47_

---

_Comment by @matt-dies-tenet3 on 2025-09-02 13:23_

Looks like the [latest release in `astral-sh/python-build-standalone`](https://github.com/astral-sh/python-build-standalone/releases/tag/20250828) includes "dynamically linked aarch64 musl builds"! 🥳 

---

_Comment by @zanieb on 2025-09-02 14:01_

Yep, thanks!

---

_Closed by @zanieb on 2025-09-02 14:01_

---

_Referenced in [kquinsland/sb820_prometheus_exporter#14](../../kquinsland/sb820_prometheus_exporter/issues/14.md) on 2025-09-02 16:25_

---

_Comment by @kopf on 2025-12-21 11:02_

> Looks like the [latest release in `astral-sh/python-build-standalone`](https://github.com/astral-sh/python-build-standalone/releases/tag/20250828) includes "dynamically linked aarch64 musl builds"! 🥳

Does this (and the fact that this issue is closed) mean that the following shouldn't happen?

```
bash-5.2$ uv self version
uv 0.9.18
bash-5.2$ uv python install 3.12
error: uv does not yet provide musl Python distributions on aarch64.
bash-5.2$ uname -a
Linux OpenWrt 6.6.73 #0 SMP Mon Feb  3 23:09:37 2025 armv7l GNU/Linux
bash-5.2$
```

Or is there another blocker to what I'm trying to do? (Install python via uv on openwrt running on a linksys wrt3200acm)

---

_Comment by @zanieb on 2025-12-21 14:17_

Hm...

```
❯ docker run --rm -it ghcr.io/astral-sh/uv:alpine sh -c "uv python install 3.12"
Installed Python 3.12.12 in 2.44s
 + cpython-3.12.12-linux-aarch64-musl (python3.12)
```

armv7l is 32-bit, right? We don't build Python for that target.

---

_Comment by @classabbyamp on 2025-12-21 16:19_

yes, armv7l != aarch64

---

_Comment by @kopf on 2025-12-21 17:30_

@zanieb, @classabbyamp, aha, okay. Sorry, my fault - the error message made me jump to google and write here before pausing to give it some thought. 

I take it there's no armv7l support planned in the future? 

---

_Comment by @zanieb on 2025-12-21 18:37_

Not that I'm aware of. Where are you running that?

We should fix the error message though.

---

_Comment by @kopf on 2025-12-21 21:44_

> Not that I'm aware of. Where are you running that?

OK, understandable. I'm running it on my router, a linksys wrt3200acm that I've installed openwrt on. The exotic setup is also a big part of the reason I'm writing here - I'd love to have `uv` managing my python installation(s) instead of leaving it to the system package manager which honestly isn't up to the task.

---

_Referenced in [astral-sh/uv#17210](../../astral-sh/uv/issues/17210.md) on 2025-12-21 21:46_

---

_Comment by @zanieb on 2025-12-21 21:47_

It looks like it's being tracked in https://github.com/astral-sh/python-build-standalone/issues/229

I'm not sure how hard it'd be to support. I presume non-trivial though.

---
