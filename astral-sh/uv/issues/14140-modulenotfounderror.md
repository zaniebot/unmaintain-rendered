```yaml
number: 14140
title: ModuleNotFoundError
type: issue
state: closed
author: LIghtJUNction
labels:
  - question
  - external
assignees: []
created_at: 2025-06-18T22:19:10Z
updated_at: 2025-06-18T22:41:15Z
url: https://github.com/astral-sh/uv/issues/14140
synced_at: 2026-01-12T16:01:43Z
```

# ModuleNotFoundError

---

_@LIghtJUNction_

### Summary

# Bug Report: ModuleNotFoundError when using `uv tool install -e .` with duplicate package configuration

## Description
When using `uv tool install -e .` with certain `pyproject.toml` configurations, the installed executable fails with `ModuleNotFoundError`, even though `uv build` works correctly in both cases.

## Expected Behavior
The executable should run normally after `uv tool install -e .` regardless of the package configuration format.

## Actual Behavior
The executable fails with `ModuleNotFoundError` in one specific configuration.

## Reproduction Steps

### Case A (Works correctly):
```toml
[tool.hatch.build.targets.wheel]
packages = ["src/astrbot"]
```
0. uv build ✔️
1. Run `uv tool install -e .`
2. Execute the script-defined executable
3. ✅ Everything works normally

### Case B (Fails):
```toml
[tool.hatch.build.targets.wheel]
packages = ["astrbot"]
```
0. uv build ✔️
1. Run `uv tool install -e .`
2. Execute the script-defined executable
3. ❌ Fails with `ModuleNotFoundError`

## Additional Information
- https://github.com/LIghtJUNction/AstrBot/tree/test
## Configuration Details
Please find the relevant `pyproject.toml` sections above. The project structure and script definitions remain the same between both cases.

### Platform

win11 amd64

### Version

 uv --version uv 0.7.9 (13a86a23b 2025-05-30) 

### Python version
Python 3.10.17

---

_Label `bug` added by @LIghtJUNction on 2025-06-18 22:19_

---

_Comment by @zanieb on 2025-06-18 22:20_

This sounds like a `hatch` / `hatchling` behavior, not anything to do with uv. 

---

_Label `bug` removed by @zanieb on 2025-06-18 22:20_

---

_Label `question` added by @zanieb on 2025-06-18 22:20_

---

_Label `external` added by @zanieb on 2025-06-18 22:20_

---

_Comment by @LIghtJUNction on 2025-06-18 22:23_

> This sounds like a `hatch` / `hatchling` behavior, not anything to do with uv.

Thank you for the clarification! You're absolutely right - this does sound like hatch/hatchling behavior rather than a uv issue. I appreciate your insight.

---

_Comment by @LIghtJUNction on 2025-06-18 22:23_

> This sounds like a `hatch` / `hatchling` behavior, not anything to do with uv.

Thank you for the clarification! I'm still learning about the relationship between these tools - could you help me understand why uv tool install would be related to hatch/hatchling behavior? I'd appreciate any insights into how these tools interact in this context.

---

_Comment by @zanieb on 2025-06-18 22:27_

When you install a package, we need to invoke a build system to create an installable distribution. Per the PEP 517 spec, uv acts as a build frontend and something else acts as a build backend. In this case, it looks like you're using `hatchling` as the build backend, hence the `[tool.hatch.build.targets.wheel]` configuration. Anything that happens within `hatchling` is out of scope for us, unless we're doing something spec in-compliant.


---

_Comment by @LIghtJUNction on 2025-06-18 22:30_

> When you install a package, we need to invoke a build system to create an installable distribution. Per the PEP 517 spec, uv acts as a build frontend and something else acts as a build backend. In this case, it looks like you're using `hatchling` as the build backend, hence the `[tool.hatch.build.targets.wheel]` configuration. Anything that happens within `hatchling` is out of scope for us, unless we're doing something spec in-compliant.

Thank you so much for the detailed explanation! Now I understand - when installing a package, uv acts as the build frontend following the PEP 517 spec and invokes the build system, while hatchling serves as the build backend handling the actual build process. So the [tool.hatch.build.targets.wheel] configuration is indeed within hatchling's domain. I get it now - anything happening within hatchling is outside of uv's control unless there's a spec compliance issue on uv's side.

Thank you again for your patience in explaining this - it really clarifies the build process for me!

---

_Closed by @zanieb on 2025-06-18 22:41_

---
