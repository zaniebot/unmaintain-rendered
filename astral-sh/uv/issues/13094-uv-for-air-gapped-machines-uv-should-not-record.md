---
number: 13094
title: "uv for air-gapped machines: uv should not record package url in uv.lock"
type: issue
state: closed
author: krupan
labels:
  - question
assignees: []
created_at: 2025-04-24T22:42:23Z
updated_at: 2025-04-26T12:46:53Z
url: https://github.com/astral-sh/uv/issues/13094
synced_at: 2026-01-10T01:25:28Z
---

# uv for air-gapped machines: uv should not record package url in uv.lock

---

_Issue opened by @krupan on 2025-04-24 22:42_

### Summary

The scenario is this:

I create a project and add packages while connected to the internet.  The packages are recorded in uv.lock along with the url of where they came from.  I copy the project (pyproject.toml and uv.lock) to another machine that has no internet connection.  I also copy all the wheels for the packages that my project needs to a directory on the offline machine (downloaded with `pip download`, because uv doesn't have that functionality).  I added this to my pyproject.toml

```
[[tool.uv.index]]
url = "/path/to/wheels/"
format = "flat"
```
When I try to `uv sync` on the offline machine, it fails because it is trying to use the url for each package that was recorded in uv.lock.  It does not fall back to using the above index, unless I either:

1. add `default = true` to the above index config
2. delete uv.lock before running `uv sync`

In both cases I get the packages I need installed into the virtual environment, but the uv.lock is then modified from the original.

It would be really, really nice to keep pyproject.toml and uv.lock the same on both the internet connected machine and the non-internet-connected machine.  Modifying either would not be necessary if the package url was not recorded in the uv.lock file and uv could get packages from whichever index has the required package in order to meet the versions specified in uv.lock.

I understand that maybe package `foo 1.2` from one source is different from `foo 1.2` from another source, but that's the sort of problem that hashes solve, not pinning a package to a single source.  `foo 1.2` downloaded on Monday could be different from `foo 1.2` downloaded on Tuesday from the same source.

### Platform

linux

### Version

uv 0.6.16 (d8ad9d3cd 2025-04-22)

### Python version

Python 3.13.3

---

_Label `bug` added by @krupan on 2025-04-24 22:42_

---

_Comment by @krupan on 2025-04-24 22:48_

Also, if for some reason you do want to pin a package to a particular package source, [uv already allows that](https://docs.astral.sh/uv/configuration/indexes/#pinning-a-package-to-an-index)

---

_Comment by @charliermarsh on 2025-04-25 01:38_

I think we're pretty unlikely to support omitting the package URL in the lockfile. uv shouldn't need to query or navigate the index in order to install your project from a lockfile.

---

_Label `bug` removed by @charliermarsh on 2025-04-25 01:39_

---

_Label `question` added by @charliermarsh on 2025-04-25 01:39_

---

_Renamed from "uv should not record package url in uv.lock" to "uv for air-gapped machines: uv should not record package url in uv.lock" by @krupan on 2025-04-25 15:17_

---

_Comment by @krupan on 2025-04-25 16:03_

Thanks for responding, and if you don't mind, I'd like to understand better.

I think what I hear you saying is that the presence of a lockfile means that uv ignores the index (and even the package list in the pyproject.toml?) because all of that is fully specified in the lock file.  But if that's the case then why does workaround 1 above cause uv to use the index to download packages and then change the lock file?

---

_Comment by @krupan on 2025-04-25 16:10_

I guess in general what I'm confused about is, it seems like uv has two different modes:

1. uv will use the information in the lock file and error out if it can't do what the lock file tells it to do
2. uv will use what little information you give it on the command-line and/or pyproject.toml and do whatever it needs to give you a working project config, and then update the lock file to record all the details of exactly what was installed and from where.

To me it feels like those different modes should be very clear and explicit in the UI.

---

_Comment by @konstin on 2025-04-25 16:10_

For the offline machine use case, you might want to consider other options, such as sharing the uv cache, using `uv export` together with `pip download` or using docker containters.

---

_Comment by @krupan on 2025-04-25 16:16_

> For the offline machine use case, you might want to consider other options, such as sharing the uv cache, using `uv export` together with `pip download` or using docker containters.

Yes, I have experimented with those workarounds (as stated in my original post here), but those all make me question why I'm even using uv.

---

_Comment by @krupan on 2025-04-26 12:46_

I'm going to open a new issue to get the above questions answered

---

_Closed by @krupan on 2025-04-26 12:46_

---

_Referenced in [astral-sh/uv#13118](../../astral-sh/uv/issues/13118.md) on 2025-04-26 13:16_

---
