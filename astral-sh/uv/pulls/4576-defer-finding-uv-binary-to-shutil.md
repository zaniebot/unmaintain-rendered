```yaml
number: 4576
title: Defer finding uv binary to shutil
type: pull_request
state: closed
author: ulucs
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-06-27T09:30:18Z
updated_at: 2024-07-19T19:25:53Z
url: https://github.com/astral-sh/uv/pull/4576
synced_at: 2026-01-10T13:42:52Z
```

# Defer finding uv binary to shutil

---

_Pull request opened by @ulucs on 2024-06-27 09:30_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently the `find_uv_binary` function searches some amount of pre-defined locations. Finding the binary itself can be achieved in a simpler way by deferring the logic to `shutil.which`, which does the following for us for free:
- handle `exe` extension logic in Windows
- search user/system-installed paths
- search virtualenv paths

In addition, leaving the location logic to `PATH` and `shutil.which` has the additional advantage of being compatible with tools that expose binaries via `PATH` (ie Nix), and any other package managers that work in ways we don't expect

## Test Plan

No new functions -- previously written tests should cover correctness. Marked as Draft until all tests pass


---

_Marked ready for review by @ulucs on 2024-06-27 09:41_

---

_Comment by @zanieb on 2024-06-27 10:52_

Hi! I don't think we want this behavior - it's only the intent of the script to find the uv binary associated with the Python module not another one on the system. If people want to find _any_ uv binary, yeah `which` is a good option to call directly. See the discussion at https://github.com/astral-sh/uv/issues/4451 for more context.

Perhaps we just need to clarify the documentation here instead.

---

_Comment by @ulucs on 2024-06-27 12:53_

All good! Is there any interest in configuring `uv` to play well with Nix? One other option is to do something similar to

```
import os

def up(file_path, times = 1):
    for i in range(times):
        file_path = os.path.dirname(file_path)
    return file_path

os.path.join(up(__file__, 5), "bin", "uv")
```

in order to target `$ROOT/bin/uv` from `$ROOT/lib/pythonXX/site-packages/uv/__init__.py`. The current approach is targeting `$ROOT/lib/pythonXX/site-packages/bin/uv` which is something I'm not sure exists

---

_Comment by @zanieb on 2024-06-27 13:35_

It'd be nice to support Nix but I don't think it makes sense for us to scan in arbitrary locations (i.e. up five levels) for the `bin`. Why doesn't Nix use one of the Python standard locations for the executable?

---

_Comment by @ulucs on 2024-06-27 14:45_

It does do that, by initializing the library root at `nix/store/$(derivation-hash)-$(python+ver)-$(package+ver)`, putting the binaries at `$ROOT/bin` and the library files at `$ROOT/lib/pythonXX/site-packages`

The problems for this case start when Python allows package location declarations via `site` and `PYTHONPATH` and disregards the binary paths. The previous version of this file had the same problem with `pip install --target` too, which was then solved by looking 2 directories up

---

_Comment by @ulucs on 2024-07-01 14:23_

Hello! Following up.

Is there any interest in supporting the python library being pulled in via `PYTHONPATH`? Or is that considered to be an unsupported use-case?

---

_Comment by @zanieb on 2024-07-03 04:55_

Hi! Sorry I'm not entirely understanding what you're proposing now. Can you clarify?

---

_Comment by @zanieb on 2024-07-19 19:25_

We want to support Nix installs but I don't think the approach here is correct. We can talk about this in an issue.

---

_Closed by @zanieb on 2024-07-19 19:25_

---
