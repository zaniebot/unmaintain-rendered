---
number: 8953
title: "MuJoCo `mjpython` fails with uvx"
type: issue
state: open
author: kevinzakka
labels:
  - bug
assignees: []
created_at: 2024-11-08T19:19:43Z
updated_at: 2025-07-07T04:55:57Z
url: https://github.com/astral-sh/uv/issues/8953
synced_at: 2026-01-07T13:12:18-06:00
---

# MuJoCo `mjpython` fails with uvx

---

_Issue opened by @kevinzakka on 2024-11-08 19:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

* uv 0.5.0 (8d665267c 2024-11-07)

[MuJoCo](https://github.com/google-deepmind/mujoco) ships with an `mjpython` executable that is only needed on MacOS (related to launching UI on main thread). Unfortunately, running `uvx --from mujoco mjpython` on macOS fails with the following error message:

```bash
failed to dlopen path '/Users/kevin/Library/Caches/uv/archive-v0/FwmQkZOQj7kyCUmF_WcgC/bin/python': dlopen(/Users/kevin/Library/Caches/uv/archive-v0/FwmQkZOQj7kyCUmF_WcgC/bin/python, 0x000A): Library not loaded: @executable_path/../lib/libpython3.11.dylib
  Referenced from: <4C4C4423-5555-3144-A177-11E83484F9AC> /Users/kevin/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/bin/python3.11
  Reason: tried: '/Users/kevin/Library/Caches/uv/archive-v0/FwmQkZOQj7kyCUmF_WcgC/lib/python3.11/site-packages/mujoco/MuJoCo_(mjpython).app/Contents/lib/libpython3.11.dylib' (no such file), '/Users/kevin/Library/Caches/uv/archive-v0/FwmQkZOQj7kyCUmF_WcgC/bin/../lib/libpython3.11.dylib' (no such file), '/libpython3.11.dylib' (no such file)
```


---

_Label `bug` added by @charliermarsh on 2024-11-09 02:19_

---

_Comment by @charliermarsh on 2024-11-09 02:19_

Interesting. Does this only happen with uv-installed Pythons?

---

_Comment by @kevinzakka on 2024-11-09 02:24_

Hey @charliermarsh, thanks for getting back so quick!

It happens both with uvx, and if I install uv with pip then do `uv pip install mujoco`. 

---

_Comment by @zanieb on 2024-11-09 14:49_

Does it happen with `uvx --python-preference only-system --from mujoco mjpython`?

---

_Comment by @kevinzakka on 2024-11-09 19:10_

Hi @zanieb, that actually works! Should I just run that all the time? Could you explain what's going on if possible?

---

_Comment by @zanieb on 2024-11-09 19:24_

> Should I just run that all the time?

You could. Though using our managed distributions has various benefits, there are some short-coming for situations like this.

> Could you explain what's going on if possible?

The distributions we include are "standalone" which means they're easy to distribute for a wide range of systems, but this means they have some quirks since it's a relatively new paradigm for Python distributions.

MuJoCo is looking for `@executable_path/../lib/libpython3.11.dylib` but it's using the wrong base path for the executable. It looks in a path relative to the virtual environment, e.g.: '/Users/kevin/Library/Caches/uv/archive-v0/FwmQkZOQj7kyCUmF_WcgC/bin/../lib/libpython3.11.dylib' but the file is actually at `~/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none/lib/libpython3.11.dylib`. There are various reasons this could be, e.g., if we are setting `home` to the wrong value in the `pyvenv.cfg` (though I thought we special-cased our own distributions).

There's sort of a similar issue in https://github.com/astral-sh/uv/issues/8879

We're looking at fixing this behavior but it's a pretty hard problem.

---

_Comment by @jonzamora on 2024-11-11 00:58_

Related issue, I encountered this a while back: https://github.com/google-deepmind/mujoco/issues/1923

I didn't spend time finding a solution to this, and reverted back to using `mamba` for package management w/ MuJoCo in the meantime.

---

_Comment by @moorage on 2025-07-07 04:55_

My workaround was just to copy or link the expected dylib:

```sh
cp ~/.local/share/uv/python/cpython-3.11.11-macos-aarch64-none/lib/libpython3.11.dylib /PATH_TO_PROJECT/.venv/bin/../lib/libpython3.11.dylib
```

---
