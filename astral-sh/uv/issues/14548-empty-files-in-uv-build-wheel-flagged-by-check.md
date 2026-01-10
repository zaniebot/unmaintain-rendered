```yaml
number: 14548
title: Empty files in uv_build wheel flagged by check-wheel-contents as being duplicates
type: issue
state: closed
author: caleb531
labels:
  - question
assignees: []
created_at: 2025-07-10T19:50:59Z
updated_at: 2025-09-26T20:53:00Z
url: https://github.com/astral-sh/uv/issues/14548
synced_at: 2026-01-10T03:23:54Z
```

# Empty files in uv_build wheel flagged by check-wheel-contents as being duplicates

---

_Issue opened by @caleb531 on 2025-07-10 19:50_

### Summary

Hi,

Apologies, the title is somewhat of a mouthful. The issue is actually pretty simple (and yes, I've checked for existing issues).

## TL;DR

Every empty file in a `uv_build`-built wheel is flagged as a duplicate by check-wheel-contents, even when they are at different paths (whereas setuptools generates a valid wheel).

## Long Answer

I am the author of the https://github.com/caleb531/automata package, which I recently migrated to uv quite successfully. Now, I am trying to migrate my build-system from setuptools to uv_build. My project is a package containing multiple sub-packages, and each level has their own `__init__.py` file.
1. When I build the wheel with setuptools, [check-wheel-contents](https://pypi.org/project/check-wheel-contents/) reports the wheel as OK.
2. However, when I build with `uv_build`, check-wheel-contents flags every empty file as a duplicate, even if they are at different paths

## Reproduction Steps

1. Run `git clone https://github.com/caleb531/automata && cd automata`
2. Run `git checkout 67fcf0b5f78b25b604611301176242caeaea5fec` (caleb531/automata@67fcf0b5f78b25b604611301176242caeaea5fec)
3. Run `uv build` to build the wheel via setuptools`
4. Run `uvx check-wheel-contents dist`; it will report the wheel as OK (i.e. valid)
6. Run `git checkout 96da40c68d1280ca83fee8cb5453ec6a94638d09` (caleb531/automata@96da40c68d1280ca83fee8cb5453ec6a94638d09)
7. Run `uv build` to build the wheel using uv_build
8. Run `uvx check-wheel-contents dist`; it will report `W002: Wheel contains duplicate files`:

```
dist/automata_lib-9.1.2-py3-none-any.whl: W002: Wheel contains duplicate files:
  automata/__init__.py
  automata/base/__init__.py
  automata/fa/__init__.py
  automata/pda/__init__.py
  automata/py.typed
  automata/regex/__init__.py
  automata/tm/__init__.py
```

At first, I thought this was a problem specifically with subpackage handling, but given that `py.typed` (also an empty file) is in this list, it seems like this might have to do with the way files are hashed irrespective of the path they are at. Because again, setuptools does not have this problem.

## build setup with setuptools (in pyproject.toml)

```toml
[build-system]
requires = ["setuptools>=77.0.3"]
build-backend = "setuptools.build_meta"

[tool.setuptools.packages.find]
where = ["."]
include = ["automata", "automata.*"]
# Explicitly exclude other top-level items
exclude = ["docs", "tests", "joss", "example_notebooks"]
```

## new build setup with uv_build (in pyproject.toml)

```toml
[build-system]
requires = ["uv_build>=0.7.19,<0.8.0"]
build-backend = "uv_build"

[tool.uv.build-backend]
module-name = "automata"
module-root = ""
```

I hope the detail and reproduction steps are helpful, but please let me know if there's any other information I can provide, or if you need me to try anything.

Thanks,
Caleb

### Platform

macOS 15.5, MacBook Air M4

### Version

uv 0.7.20 (251420396 2025-07-09)

### Python version

Python 3.12.11

---

_Label `bug` added by @caleb531 on 2025-07-10 19:51_

---

_Comment by @charliermarsh on 2025-07-10 19:56_

Interesting, thanks for the report!

---

_Assigned to @konstin by @zanieb on 2025-07-10 22:40_

---

_Comment by @konstin on 2025-07-17 12:05_

Is there a problem with the wheel from uv_build? Having multiple empty `__init__.py` is normal for Python projects, I'm not following why check-wheel-contents is raising an alarm about this.

---

_Unassigned @konstin by @konstin on 2025-08-08 14:07_

---

_Label `bug` removed by @konstin on 2025-08-08 14:07_

---

_Label `question` added by @konstin on 2025-08-08 14:07_

---

_Comment by @caleb531 on 2025-09-26 20:53_

@konstin Apologies, just saw your comment. But good news!

I am no longer able to reproduce this issue in the latest version of uv_build (0.8.22). I can still reproduce it in the latest 0.7.x release (0.7.22). 

I've run the following code against all versions between 0.7.22 and 0.8.22, and 0.8.13 is the first one where the issue is no longer reproducible. Looking through the release notes, it seems that #15400 has definitively fixed this issue. Apparently, #15398 also highlights the underlying issue.

I'll be upgrading all my projects to use this new version of uv_build, so I will be closing the issue now.

---

_Closed by @caleb531 on 2025-09-26 20:53_

---
