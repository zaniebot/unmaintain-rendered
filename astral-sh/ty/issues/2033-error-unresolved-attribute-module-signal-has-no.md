```yaml
number: 2033
title: "error[unresolved-attribute]: Module `signal` has no member `CTRL_C_EVENT`"
type: issue
state: closed
author: kracekumar
labels: []
assignees: []
created_at: 2025-12-17T20:33:36Z
updated_at: 2025-12-18T00:51:51Z
url: https://github.com/astral-sh/ty/issues/2033
synced_at: 2026-01-12T15:54:26Z
```

# error[unresolved-attribute]: Module `signal` has no member `CTRL_C_EVENT`

---

_@kracekumar_

### Summary

I am trying to replace mypy with ty in litecli library. The `signal`in stdlib module has platform specific code. ty is unable to figure this out.

Code

```python

def send_ctrl_c_to_pid(pid, wait_seconds):
    """Sends a Ctrl-C like signal to the given `pid` after `wait_seconds`
    seconds."""
    time.sleep(wait_seconds)
    system_name = platform.system()
    if system_name == "Windows":
        os.kill(pid, signal.CTRL_C_EVENT)
    else:
        os.kill(pid, signal.SIGINT)
```

Diagnostics

```
error[unresolved-attribute]: Module `signal` has no member `CTRL_C_EVENT`
  --> tests/utils.py:81:22
   |
79 |     system_name = platform.system()
80 |     if system_name == "Windows":
81 |         os.kill(pid, signal.CTRL_C_EVENT)
   |                      ^^^^^^^^^^^^^^^^^^^
82 |     else:
83 |         os.kill(pid, signal.SIGINT)
   |
info: The member may be available on other Python versions or platforms
info: Python 3.9 was assumed when resolving the `CTRL_C_EVENT` attribute
  --> pyproject.toml:96:18
   |
95 | [tool.ty.environment]
96 | python-version = "3.9"
   |                  ^^^^^ Python version configuration
97 | root = ["litecli"]
   |
info: rule `unresolved-attribute` is enabled by default
```
mypy in the playground: https://mypy-play.net/?mypy=latest&python=3.12&gist=522d6c41ceace3c5f037d4543a3e4e6b
ty playground: https://play.ty.dev/8d9108ff-0369-4a19-bfb1-734a4d125648

This issue is similar to https://github.com/astral-sh/ty/issues/1990

### Version

ty 0.0.2 (42835578d 2025-12-16)
Python: 3.9

---

_Comment by @carljm on 2025-12-17 20:55_

Thanks for the report! There are a few inter-related things happening here.

1. ty doesn't understand `system_name = platform.system(); if system_name == "Windows":` as establishing a platform-guarded section of code. No other type checker understands this either. The form that all type-checkers understand is `if sys.platform == "win32"` -- switching to that [fixes the issue](https://play.ty.dev/55589b3d-6012-4d56-86be-3d20b6c8c3cb).
2. I think both pyright and mypy playgrounds default to platform "all"; we currently default to `linux` in the playground. (Probably we should also default to `"all"` instead.) Mypy playground doesn't seem to support setting a specific platform. If you set pyright playground to Linux or Darwin platform, in strict mode it gives the same `unresolved-attribute` error that we do. Locally we default to the platform you are running on; I'm not sure how mypy/pyright default it.
3. If you add `"python-platform": "all"` in the ty config, [the error switches](https://play.ty.dev/82aa9027-2ab0-47cb-bac4-3bd166ebfd11) from `[unresolved-attribute]` to a `[possibly-missing-attribute]` warning. (If we are checking for all platforms, we still want to warn you that the attribute may not exist on some platforms.) This is a warning that mypy and pyright do not provide. If you don't want it, you can suppress it, either on that specific line, or by disabling the warning entirely in your config.

I think ty is working as expected here, so I'll close this issue for now.

---

_Closed by @carljm on 2025-12-17 20:55_

---

_Comment by @AlexWaygood on 2025-12-17 21:03_

> 2\. I think both pyright and mypy playgrounds default to platform "all"; we currently default to `linux` in the playground. (Probably we should also default to `"all"` instead.)

Oh, i thought they defaulted to the platform they were being executed on, like us. I might be wrong though

---

_Comment by @carljm on 2025-12-17 21:13_

> Oh, i thought they defaulted to the platform they were being executed on, like us

I was specifically talking here about their playgrounds, which seem to default to "all" (at least pyright's explicitly does, mypy I was just guessing based on behavior).

---

_Comment by @kracekumar on 2025-12-18 00:51_

Thanks for the deep dive here!:-)  I think option one should work `if sys.platform == "win32"`. I agree it is useful to warn against other platforms since it's good to catch these different behaviors early. 

---
