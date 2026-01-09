---
number: 12047
title: uv uses stale project pth file when switching from flat to src layout
type: issue
state: closed
author: dalukes-csob-cz
labels:
  - bug
  - cache
assignees: []
created_at: 2025-03-07T15:32:45Z
updated_at: 2025-03-17T21:56:11Z
url: https://github.com/astral-sh/uv/issues/12047
synced_at: 2026-01-07T13:12:18-06:00
---

# uv uses stale project pth file when switching from flat to src layout

---

_Issue opened by @dalukes-csob-cz on 2025-03-07 15:32_

### Summary

I inherited a project with flat layout, migrated it to uv, added a few `project.scripts`, then decided to switch to a src layout, at which point the scripts stopped working.

After fiddling around a little bit, I noticed that the project's pth file in the virtualenv wasn't reflecting the new src layout, even if I recreated the virtualenv. This tipped me off it might be a cache issue, and indeed, when I cleared the cache, everything started working again.

Here's a Containerfile with an MRE:

<details>


```Containerfile
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:0.6.5-debian-slim

RUN <<-EOF
echo 'INIT PROJECT WITH FLAT LAYOUT + PROJECT SCRIPT ----------------------------------'
uv init mre
cd mre
echo '
[project.scripts]
main = "mre.main:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
' >>pyproject.toml
mkdir mre
touch mre/__init__.py
mv -t mre main.py

echo
echo 'RUNNING PROJECT SCRIPT WORKS FINE -----------------------------------------------'
uv run main

echo
echo 'PTH FILE IS CORRECT -------------------------------------------------------------'
cat .venv/lib/python*/site-packages/_mre.pth
echo

echo
echo 'SWITCH TO SRC LAYOUT ------------------------------------------------------------'
mkdir src
mv -t src mre

echo
echo 'RECREATE VENV WITH CACHE --------------------------------------------------------'
rm -rf .venv
uv sync

echo
echo 'RUNNING PROJECT SCRIPT FAILS ----------------------------------------------------'
uv run main

echo
echo 'PTH FILE IS STALE FROM CACHE ----------------------------------------------------'
cat .venv/lib/python*/site-packages/_mre.pth
echo

echo
echo 'RECREATE VENV WITHOUT CACHE -----------------------------------------------------'
uv cache clean
rm -rf .venv
uv sync

echo
echo 'RUNNING PROJECT SCRIPT WORKS FINE AGAIN -----------------------------------------'
uv run main

echo
echo 'PTH FILE HAS BEEN UPDATED FOR SRC LAYOUT ----------------------------------------'
cat .venv/lib/python*/site-packages/_mre.pth
echo
EOF
```

</details>

Would it be possible for uv to invalidate the cache entry for the project package in case of layout changes of this kind? I guess part of the problem might be that the src vs flat layout is autodetected, so they both work with no changes to `pyproject.toml`, which may be why the cache is not getting invalidated...?

And thanks so much for uv, it's awesome :)

### Platform

macOS 15.3.1 (24D70) arm64

### Version

uv 0.6.5 (Homebrew 2025-03-06)

### Python version

Whatever uv in container picks (CPython 3.13.2 as of writing)

---

_Label `bug` added by @dalukes-csob-cz on 2025-03-07 15:32_

---

_Comment by @charliermarsh on 2025-03-07 15:52_

This has come up before though I'm struggling to find the issue... Your diagnosis is correct, though: we cache based on the `pyproject.toml`, and so if the `pyproject.toml` doesn't change, we don't invalidate the cache automatically. We could consider adding `src` as a cache key so that if a `src` directory is added or removed, we invalidate the cache, which would at least help with this common case?


---

_Label `cache` added by @charliermarsh on 2025-03-07 15:52_

---

_Comment by @dalukes-csob-cz on 2025-03-07 16:12_

Thanks for the quick reply!

> We could consider adding src as a cache key so that if a src directory is added or removed, we invalidate the cache, which would at least help with this common case?

Sounds like a good idea as far as I'm concerned :) More exotic layout changes should typically be reflected in explicit config changes within `pyproject.toml` anyway, see e.g. https://hatch.pypa.io/1.13/config/build/#packages.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-08 02:57_

---

_Referenced in [astral-sh/uv#12062](../../astral-sh/uv/pulls/12062.md) on 2025-03-08 02:57_

---

_Comment by @tpgillam on 2025-03-14 23:50_

> This has come up before though I'm struggling to find the issue...

Sounds a bit like https://github.com/astral-sh/uv/issues/10390 perhaps — though if so I'm sure that's not the only one!



---

_Closed by @charliermarsh on 2025-03-17 21:56_

---
