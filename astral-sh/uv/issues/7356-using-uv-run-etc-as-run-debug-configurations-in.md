---
number: 7356
title: "Using \"uv run\" etc as Run/Debug configurations in PyCharm"
type: issue
state: closed
author: tmct
labels:
  - external
assignees: []
created_at: 2024-09-13T10:59:00Z
updated_at: 2025-12-18T09:36:24Z
url: https://github.com/astral-sh/uv/issues/7356
synced_at: 2026-01-10T01:24:14Z
---

# Using "uv run" etc as Run/Debug configurations in PyCharm

---

_Issue opened by @tmct on 2024-09-13 10:59_

Hi,

`uv run` is extremely useful, I use it to launch scripts with a couple of dependencies with the minimum of virtual environment creation faff.

Sometimes I would like to debug the same script right then and there in PyCharm, but it doesn' seem to work out of the box: it doesn't seem to attach the debugger to the Python process when I create "uv run" as a shell script Run Configuration -  I suppose that "uv" is not running as a Python process... Can you think of a good way of achieving this, or would I need to request that JetBrains make some official uv tie-in?

Thanks,
Tom

---

_Comment by @konstin on 2024-09-13 15:03_

Hi! We hope for native uv support in PyCharm eventually (https://youtrack.jetbrains.com/issue/PY-70533/Support-package-management-via-uv).

As a workaround, you can use a regular Python run configuration and call `uv sync` as a before launch external tool.

---

_Comment by @JalinWang on 2025-02-28 02:38_

FYI: Now uv can be used to manage packages [1] [2] while the `uv run` integration is still on the way [3].

[1] The JetBrains blog: [PyCharm 2024.3.2: uv Package Management Support and More!](https://blog.jetbrains.com/pycharm/2025/01/pycharm-2024-3-2/)

[2] The YouTrack issue:  https://youtrack.jetbrains.com/issue/PY-70533/Support-package-management-via-uv

[3] A comment from [2]:
> For the uv run, please check https://youtrack.jetbrains.com/issue/PY-79031/Add-uv-run-as-a-run-configuration

---

_Label `external` added by @konstin on 2025-02-28 11:58_

---

_Comment by @konstin on 2025-12-15 16:50_

Native `uv run` support has landed in PyCharm 2025.3:

<img width="1353" height="934" alt="Image" src="https://github.com/user-attachments/assets/80465fce-f85a-490a-9ea1-a787a8064d11" />

It also includes a warning if the dependencies aren't fresh:

<img width="889" height="215" alt="Image" src="https://github.com/user-attachments/assets/9c627833-12a4-43ab-a8bd-eb6008d40ea0" />

---

_Closed by @konstin on 2025-12-15 16:50_

---

_Comment by @zmeir on 2025-12-17 12:48_

Hey, I don't know if this is the place to write this, but the new `uv run` option in the PyCharm run configuration doesn't take into account the proxy configuration from the PyCharm settings.

So when PyCharm uses `uv run` behind e.g. a corporate proxy it won't set up the http proxy, which might cause the command to fail (like when it needs to pull a build backed but without the proxy the corporate firewall blocks it).

Specifically, I think this only happens when a build-backend is required.

Example:

pyproject.toml
```toml
[project]
name = "uv-run-example"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.13"
dependencies = []

[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[tool.hatch.build]
packages = ["."]
```

main.py
```
print("Hello World")
```

With PyCharm uv run configuration I get the following output:

<img width="936" height="665" alt="Image" src="https://github.com/user-attachments/assets/dbaa3e73-c2c9-4641-8a9a-8a69bb0d8556" />

```
/Users/zmeir/.local/bin/uv run /Users/zmeir/git/uv-run-example/.venv/bin/python /Users/zmeir/git/uv-run-example/main.py 
   Building uv-run-example @ file:///Users/zmeir/git/uv-run-example
  × Failed to build `uv-run-example @ file:///Users/zmeir/git/uv-run-example`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `hatchling`
  ├─▶ Failed to fetch: `https://pypi.org/simple/hatchling/`
  ├─▶ Request failed after 3 retries
  ├─▶ error sending request for url (https://pypi.org/simple/hatchling/)
  ╰─▶ operation timed out

Process finished with exit code 1
```

And when I set the proxy in the run configuration (although it is already defined in my PyCharm settings) it works:

<img width="1522" height="663" alt="Image" src="https://github.com/user-attachments/assets/c753178c-cae3-40c2-b8c0-abbdb30d8575" />

```
/Users/zmeir/.local/bin/uv run /Users/zmeir/git/uv-run-example/.venv/bin/python /Users/zmeir/git/uv-run-example/main.py 
   Building uv-run-example @ file:///Users/zmeir/git/uv-run-example
      Built uv-run-example @ file:///Users/zmeir/git/uv-run-example
Installed 1 package in 6ms
Hello World

Process finished with exit code 0
```

Also works when I tell PyCharm to run without uv run:

<img width="936" height="665" alt="Image" src="https://github.com/user-attachments/assets/a7209c73-d6d0-4a4b-80aa-3c28c9361db0" />

```
/Users/zmeir/git/uv-run-example/.venv/bin/python /Users/zmeir/git/uv-run-example/main.py 
Hello World

Process finished with exit code 0
```

---

_Comment by @konstin on 2025-12-17 15:20_

Can you confirm that those are the correct proxy settings for uv, and that the terminal has the same proxy settings? I tested it with a modified uv build that dumps the env vars and it does pass the env var for me and uv fails when setting an incorrect proxy and `UV_NO_CACHE=1`.

<img width="1439" height="419" alt="Image" src="https://github.com/user-attachments/assets/9f6f42ac-ee80-431d-9f6c-c0d08743fc39" />

---

_Comment by @gmitricheva on 2025-12-18 07:22_

@konstin we will be deprecating the separate 'uv run' run configuration you're checking. Please use the 'Python' run configuration with or without 'run with uv run' parameter, as shown on zmeir's screenshots.

I've created a bug in our tracker https://youtrack.jetbrains.com/issue/PY-86413/run-with-uv-run-does-not-pick-up-proxy-settings
Please comment there if it proves to be a problem on the uv side (which it doesn't seem to be at the moment).

Galina Mitricheva
PyCharm team


---

_Comment by @konstin on 2025-12-18 09:35_

Thanks for the heads up!

I tried looking at the bug report, is it non-public?

<img width="1267" height="611" alt="Image" src="https://github.com/user-attachments/assets/9b90f158-7e59-4876-8d4a-4cc8a9076540" />

---

_Comment by @gmitricheva on 2025-12-18 09:36_

Oops, was not intended as such. Opened to all now

---
