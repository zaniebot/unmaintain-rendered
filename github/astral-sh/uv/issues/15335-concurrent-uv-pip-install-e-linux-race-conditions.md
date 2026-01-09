---
number: 15335
title: "Concurrent `uv pip install -e` Linux race conditions"
type: issue
state: open
author: charlesnicholson
labels:
  - bug
assignees: []
created_at: 2025-08-18T00:52:51Z
updated_at: 2025-08-18T01:14:59Z
url: https://github.com/astral-sh/uv/issues/15335
synced_at: 2026-01-07T13:12:19-06:00
---

# Concurrent `uv pip install -e` Linux race conditions

---

_Issue opened by @charlesnicholson on 2025-08-18 00:52_

### Summary

We have an extremely parallelized build that invokes `uv` from a bunch of different hardware threads, and we carefully manage the dependencies between them such that all required commands for a target are completed before that target builds.

We upgraded to uv 0.8.5, and more recently 0.8.11, and have started seeing what looks like a race condition, but only on Linux. Reducing our build system's allowed concurrent executions of `uv` from 128 to 1 on Linux fixed it. It never manifests on macOS or Windows, which makes me suspect something like uv not flushing all pending file io before exiting? We've never seen this failure over dozens of runs across macOS + Windows + Linux per day for well over a year now, until we upgraded to the 0.8 line.

Anyway, I don't have the time to put together a small MRE right now, but might be able to in the future.

The job graph looks roughly like:

`uv venv path/to/venv`
`uv pip install -p path/to/venv -r path/to/requirements.txt`

and then any number of concurrent invocations of

`uv pip install -p path/to/venv -e path/to/package --config-settings editable_mode=compat -c path/to/constraints.txt`

The error that arises from one of these `uv pip install ... -e` lines looks like this:
```
../external/vendor/uv/linux-x64/uv pip install -e path/to/my.leaf-package --config-settings editable_mode=compat -c ../build/constraints-linux.txt
error: Request failed after 1 retries
  Caused by: Failed to fetch: `[https://pypi.org/simple/my-core-package/`](https://pypi.org/simple/my-core-package/%60)
  Caused by: HTTP status client error (404 Not Found) for url (https://pypi.org/simple/my-core-package/)
```

The error is that uv couldn't download `my.core-package` from pypi, but what's actually happening is that uv doesn't think that `my.core-package` exists in the venv! `my.leaf-package`'s pyproject.toml lists `my.core-package` as a dependency, and our build system ensures that `my.core-package` has been editable-installed into the venv and that the uv process has exited before it runs the editable-install command for `my.leaf-package`.

I also think that in this example, the failure happened long before the attempt to install `my.leaf-package`; the build logs show that 2 uv editable installations occurred at almost the exact same time:
```
[4/419] UV //build/venv:venv__venv(//gntools/toolchains:gcc)
[5/419] UV //build/venv:venv__requirements(//gntools/toolchains:gcc)
[9/419] UV //my.core-package:my.core-package__editable_install(//gntools/toolchains:gcc)
[12/419] UV //my.other-package:my.other-package__editable_install(//gntools/toolchains:gcc)
```

I suspect that the simultaneous installations at 9 and 12 here are leaving uv in a state where it doesn't "see" that `my.core-package` has been installed to the venv. The `my.leaf-package` step doesn't run for a good ~30 seconds after "core" and "other", so the venv / cache are likely left in a bad state in this case.

The GitHub Actions runner we're using is `ubuntu-latest`, which at time of writing is:

```
Operating System
  Ubuntu
  24.04.2
  LTS
Runner Image
  Image: ubuntu-24.04
  Version: 20250810.1.0
  Included Software: https://github.com/actions/runner-images/blob/ubuntu24/20250810.1/images/ubuntu/Ubuntu2404-Readme.md
  Image Release: https://github.com/actions/runner-images/releases/tag/ubuntu24%2F20250810.1
```

### Platform

Ubuntu 24.04.2 LTS

### Version

uv 0.8.5, uv 0.8.11

### Python version

python 3.13

---

_Label `bug` added by @charlesnicholson on 2025-08-18 00:52_

---
