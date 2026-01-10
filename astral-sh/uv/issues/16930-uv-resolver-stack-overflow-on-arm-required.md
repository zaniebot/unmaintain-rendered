---
number: 16930
title: uv-resolver stack overflow on ARM required-environments
type: issue
state: open
author: Oblynx
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-12-02T13:39:48Z
updated_at: 2025-12-02T14:00:07Z
url: https://github.com/astral-sh/uv/issues/16930
synced_at: 2026-01-10T01:26:11Z
---

# uv-resolver stack overflow on ARM required-environments

---

_Issue opened by @Oblynx on 2025-12-02 13:39_

### Summary

I have a uv workspace project. In my project, I declare:
```
[tool.uv]
# Ensure resolution for supported environments
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'x86_64'",
]
```
and `uv lock` successfully compiles the dependencies.

However, if I switch to **aarch64**, there's a stack overflow:
- `sys_platform == 'linux' and platform_machine == 'x86_64'` => 🟢 
- `sys_platform == 'linux' and platform_machine == 'aarch64'`=> 🔴 
- `sys_platform == 'darwin' and platform_machine == 'arm64'` => 🟢
- `sys_platform == 'win32' and platform_machine == 'AMD64'`  => 🔴

Values that AFAIK shouldn't mean anything actually work:
- `sys_platform == 'linux' and platform_machine == 'HELLO'`  => 🟢 
- `sys_platform == 'linux' and platform_machine == 'arm64'`  => 🟢

Also note that the same values in `environments` (instead of `required-environments`) work as well.


### Logs

Note that I set the env var `UV_EXTRA_INDEX_URL`.

```
[tool.uv]
required-environments = [
    "sys_platform == 'linux' and platform_machine == 'aarch64'",
]
```

Depending on the combination of environments and required-environments, the logs change.

```
DEBUG Found fresh response for: https://gitlab.com/api/v4/groups/106570154/-/packages/pypi/simple/send2trash/
DEBUG Searching for a compatible version of cadquery{platform_machine == 'x86_64'} (>=2.5.2, <2.6.dev0)
DEBUG Selecting: cadquery==2.5.2 [preference] (cadquery-2.5.2-py3-none-any.whl)
DEBUG Found fresh response for: https://gitlab.com/api/v4/groups/106570154/-/packages/pypi/simple/sqlalchemy/
DEBUG Adding transitive dependency for cadquery==2.5.2: cadquery==2.5.2
DEBUG Adding transitive dependency for cadquery==2.5.2: cadquery{platform_machine == 'x86_64'}==2.5.2
DEBUG Searching for a compatible version of cadquery (==2.5.2)
DEBUG Selecting: cadquery==2.5.2 [preference] (cadquery-2.5.2-py3-none-any.whl)
DEBUG Adding transitive dependency for cadquery==2.5.2: cadquery-ocp>=7.7.0, <7.8
...
DEBUG Searching for a compatible version of cadquery-ocp{platform_machine == 'x86_64'} (>=7.7.2.2b2, <7.7.3.dev0)
DEBUG Found fresh response for: https://gitlab.com/api/v4/groups/106570154/-/packages/pypi/simple/werkzeug/
...
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7c/50/11c9ee1ede64b45d687fd36eb8768dafc57afc78b4d83396920cfd69ed30/path-17.1.1-py3-none-any.whl.metadata

thread 'uv-resolver' (30456) has overflowed its stack
fatal runtime error: stack overflow, aborting
Aborted (core dumped)
```

### Platform

ubuntu 24.04 x86_64

### Version

uv 0.9.13

### Python version

Python 3.11.14

---

_Label `bug` added by @Oblynx on 2025-12-02 13:39_

---

_Label `needs-mre` added by @konstin on 2025-12-02 13:41_

---

_Comment by @konstin on 2025-12-02 13:41_

Can you provide a set of dependencies that reproduces this problem? We haven't seen this crash before, so otherwise I wouldn't know how to reproduce it.

---

_Comment by @Oblynx on 2025-12-02 13:42_

You believe that it's dependency specific?

---

_Comment by @konstin on 2025-12-02 13:44_

Yes, we know that `sys_platform == 'linux' and platform_machine == 'aarch64'` works in other configurations, we even have forms of this marker (normalized to `platform_machine == 'aarch64' and sys_platform == 'linux'`) in our test suite, there needs to be something specific about the dependency structure for it to fail this way.

---

_Comment by @Oblynx on 2025-12-02 13:56_

Here's the set of package constraints I had to remove for the resolution to succeed (+ delete the existing uv.lock):
```
-    "vtk~=9.2.6",
-    "pyacvd",
-    "cadquery~=2.5.2 ; platform_machine == 'x86_64'",
-    "cadquery-ocp~=7.7.2.2b2; platform_machine == 'x86_64'",
```

Each of these packages causes the crash.

---

_Comment by @Oblynx on 2025-12-02 14:00_

Interestingly, if I make a minimal project with just these dependencies, I don't see the crash.

---
