---
number: 1779
title: uv fails to find python in non-venv install on Windows
type: issue
state: closed
author: gaborbernat
labels:
  - windows
assignees: []
created_at: 2024-02-20T19:35:40Z
updated_at: 2024-02-28T02:10:30Z
url: https://github.com/astral-sh/uv/issues/1779
synced_at: 2026-01-07T13:12:16-06:00
---

# uv fails to find python in non-venv install on Windows

---

_Issue opened by @gaborbernat on 2024-02-20 19:35_

When trying to run the tool on Windows, I get this rust like stack trace:

```
Error: The system cannot find the file specified.
 (from CreateProcessA(null(), child_cmdline.into_bytes_with_nul().as_mut_ptr(),
    null(), null(), 1, 0, null(), null(), addr_of!(*si),
    child_process_info.as_mut_ptr()))
```

See https://github.com/tox-dev/tox-uv/actions/runs/7978647392/job/21784221224?pr=19

I'm using uv find_uv_bin...

---

_Referenced in [tox-dev/tox-uv#19](../../tox-dev/tox-uv/pulls/19.md) on 2024-02-20 20:12_

---

_Comment by @AlexWaygood on 2024-02-20 21:09_

Interesting, this is the same error I was getting in #1639 (cc. @MichaReiser)

---

_Comment by @AlexWaygood on 2024-02-20 21:14_

Have you created and activated a virtual environment? My and Micha's conclusion from debugging #1639 was that it's not possible on Windows (yet) to use the "trick" where you pretend a virtual environment has been created and activated by doing the `echo "VIRTUAL_ENV=${Python_ROOT_DIR}" >> $GITHUB_ENV` in a GitHub Actions workflow. The reason why this doesn't work on Windows is likely due to the fact that `python.exe` is found in a different directory on Windows when a virtual environment hasn't been activated (it's _not_ in a `Scripts/` directory when a virtual environment hasn't been activated, and `uv` assumes `python.exe` will always be in a `Scripts/` directory on Windows). So the `VIRTUAL_ENV` trick to avoid having to create and activate an environment currently only works on Unix.

(Edit: it looks like you _are_ indeed doing the Unix-only "trick": https://github.com/tox-dev/tox-uv/blob/a93e37e98a012d07d6ac151e097c444a77d15b0a/.github/workflows/check.yml#L43)

---

_Comment by @AlexWaygood on 2024-02-20 21:22_

I outlined a cross-platform way of using `uv` in a GitHub Actions workflow here: https://github.com/astral-sh/uv/issues/1386#issuecomment-1952420541

---

_Comment by @gaborbernat on 2024-02-20 21:25_

Yeah, I can do that, but sounds more like a workaround than a solution for the underlying problem 👍 

---

_Comment by @AlexWaygood on 2024-02-20 21:28_

> Yeah, I can do that, but sounds more like a workaround than a solution for the underlying problem 👍

Oh of course. Longer-term we want to have first-class support for GitHub Actions, which I think will probably include something like #1526. We absolutely agree the current situation is far from ideal :)

---

_Label `question` added by @zanieb on 2024-02-21 00:20_

---

_Label `windows` added by @MichaReiser on 2024-02-21 06:39_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-21 06:39_

---

_Referenced in [astral-sh/uv#1803](../../astral-sh/uv/pulls/1803.md) on 2024-02-21 11:21_

---

_Unassigned @MichaReiser by @MichaReiser on 2024-02-21 15:31_

---

_Comment by @jenshnielsen on 2024-02-22 08:14_

I ran into this error when trying to run pytest installed with uv from a pre existing conda environment. 

---

_Comment by @MichaReiser on 2024-02-22 09:40_

The main issue here is that `uv` assumes that Python is located at `$VIRTUAL_ENV/Scripts/python.exe` which isn't the case when pointing to a global python installation. see https://github.com/astral-sh/uv/pull/1803 for some details.

@jenshnielsen https://github.com/astral-sh/uv/pull/1803 might help with your issue, depending on the layout of the `venv` directory

---

_Assigned to @MichaReiser by @konstin on 2024-02-22 10:14_

---

_Unassigned @MichaReiser by @MichaReiser on 2024-02-22 12:07_

---

_Renamed from "uv crashes on Windows with The system cannot find the file specified" to "uv fails to find python in non-venv install" by @konstin on 2024-02-22 12:10_

---

_Label `question` removed by @konstin on 2024-02-22 12:11_

---

_Renamed from "uv fails to find python in non-venv install" to "uv fails to find python in non-venv install on Windows" by @AlexWaygood on 2024-02-22 21:45_

---

_Comment by @Pixel-Minions on 2024-02-23 17:16_

I wonder of this is related to the same issue I am having with embedded versions of Python:
https://github.com/astral-sh/uv/issues/1656

---

_Comment by @mkleinbort on 2024-02-26 14:26_

Is a solution for this in the works? I'm preparing an internal talk on UV for users that use windows and keen to know what they should expect. 

---

_Comment by @MichaReiser on 2024-02-26 14:28_

We're planning to solve this as part of https://github.com/astral-sh/uv/issues/1526

The recommended local workflow is to use a venv instead of installing dependencies globally.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-26 14:54_

---

_Comment by @charliermarsh on 2024-02-26 14:55_

Yup I'm on it.

---

_Referenced in [astral-sh/uv#1988](../../astral-sh/uv/issues/1988.md) on 2024-02-26 18:13_

---

_Referenced in [astral-sh/uv#2000](../../astral-sh/uv/pulls/2000.md) on 2024-02-27 01:56_

---

_Closed by @charliermarsh on 2024-02-28 02:10_

---

_Referenced in [astral-sh/uv#6008](../../astral-sh/uv/issues/6008.md) on 2024-08-11 14:44_

---
