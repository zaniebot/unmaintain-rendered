---
number: 4088
title: A path dependency is incompatible with the current platform
type: issue
state: closed
author: henryiii
labels: []
assignees: []
created_at: 2024-06-06T07:12:01Z
updated_at: 2025-03-05T18:13:48Z
url: https://github.com/astral-sh/uv/issues/4088
synced_at: 2026-01-07T13:12:17-06:00
---

# A path dependency is incompatible with the current platform

---

_Issue opened by @henryiii on 2024-06-06 07:12_

I'm running into `error: Failed to determine installation plan Caused by: A path dependency is incompatible with the current platform` when trying to use uv in cibuildwheel for macOS and Windows wheels (Linux is fine).

Windows fails right away with `deflate-0.8.0-cp38-cp38-win32.whl`; I'd speculate it might be a 32-bit Windows issue.

MacOS Intel is fine for CPython 3.8-3.10 then 3.11 fails on `deflate-0.8.0-cp311-abi3-macosx_10_9_x86_64.whl`. 3.8-3.10 were not ABI3 wheels in this case, though, so it seems to be the ABI3 wheel.

MacOS ARM dies sooner, on `deflate-0.8.0-cp39-cp39-macosx_11_0_arm64.whl`, which is the first tested wheel (3.8 can't be tested on ARM since we use the Intel CPython install).

This might be a regression, as I know the Windows one worked a week or two ago.

---

_Comment by @henryiii on 2024-06-06 07:38_

I might be using the wrong Python when setting up the venv, let me reopen if I pin it down to uv again.

---

_Closed by @henryiii on 2024-06-06 07:38_

---

_Comment by @henryiii on 2024-06-06 07:45_

Adding `--python=python` when making the venv seems to have fixed it.

---

_Comment by @munechika-koyo on 2025-03-05 16:23_

I also saw the above error in building virtual env at test phase at https://github.com/munechika-koyo/cherab_lhd/actions/runs/13680318668/job/38250721212.
Here I think the below line has somthing to do with the issue:
```
uv venv /private/var/folders/t8/r_s4tkhn46g6y0d15z25l36c0000gn/T/cibw-run-zf_mu_np/cp310-macosx_x86_64/venv-test-x86_64 --python=/Library/Frameworks/Python.framework/Versions/3.10/bin/python3
```

@henryiii Could you please tell me how you configured `--python` argument in the setup file (e.g. pyproject.toml)?

---

_Comment by @henryiii on 2025-03-05 18:13_

You are hardcoding a minimum macOS target of macOS 14 but trying to build on macOS 13.

---
