---
number: 11461
title: "`ModuleNotFoundError` when attempting to use certain `tool.uv.build-backend` settings"
type: issue
state: closed
author: chrisrodrigue
labels:
  - question
  - preview
assignees: []
created_at: 2025-02-12T21:57:59Z
updated_at: 2025-03-17T18:13:48Z
url: https://github.com/astral-sh/uv/issues/11461
synced_at: 2026-01-07T13:12:18-06:00
---

# `ModuleNotFoundError` when attempting to use certain `tool.uv.build-backend` settings

---

_Issue opened by @chrisrodrigue on 2025-02-12 21:57_

### Summary

I've noticed a recurring non-descript error message that happens when trying to set some of the backend settings like `tool.uv.build-backend.source-includes` and  `tool.uv.build-backend.data`. 

The only setting that doesn't cause this error to be thrown is `tool.uv.build-backend.module-root`.

```
uv pip install --no-build-isolation --offline -e .
```
```cmd
  × Failed to build `albatross @ file:///C:/Users/user/dev/albatross`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv.build_editable` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
          import uv as backend
      ModuleNotFoundError: No module named 'uv'
```

`uv` is available on `PATH` so I'm not sure what module the error is referring to. Is this looking for a rust crate, or a python uv build backend module?

### Platform

Windows 11

### Version

uv 0.5.30

### Python version

Python 3.13.2

---

_Label `bug` added by @chrisrodrigue on 2025-02-12 21:58_

---

_Referenced in [astral-sh/uv#11382](../../astral-sh/uv/issues/11382.md) on 2025-02-12 21:58_

---

_Label `preview` added by @charliermarsh on 2025-02-12 21:59_

---

_Comment by @charliermarsh on 2025-02-12 22:00_

\cc @konstin 

---

_Assigned to @konstin by @konstin on 2025-02-12 22:53_

---

_Comment by @konstin on 2025-02-13 13:51_

To build with uv, you don't only need the binary, but also the Python package for PEP 517 integration. The uv Python ships both the binary and installs a `uv` module into the venv. For building with `--no-build-isolation`, the `uv` python package (`uv pip install uv`) is required, otherwise we don't have the PEP 517.

The behavior should currently depending on whether preview is activated, the build backend does has a direct build mode that is a fast path that skips the PEP 517 python shims when we know that we're building a package that uses uv. This can interact oddly with `--no-build-isolation`, which is an option that only affects PEP 517. If you want to force a PEP 517 build, you can use `uv build --force-pep517`.

---

_Label `bug` removed by @konstin on 2025-02-13 13:51_

---

_Label `question` added by @konstin on 2025-02-13 13:51_

---

_Comment by @chrisrodrigue on 2025-02-13 18:15_

> To build with uv, you don't only need the binary, but also the Python package for PEP 517 integration. The uv Python ships both the binary and installs a `uv` module into the venv. For building with `--no-build-isolation`, the `uv` python package (`uv pip install uv`) is required, otherwise we don't have the PEP 517.
> 
> The behavior should currently depending on whether preview is activated, the build backend does has a direct build mode that is a fast path that skips the PEP 517 python shims when we know that we're building a package that uses uv. This can interact oddly with `--no-build-isolation`, which is an option that only affects PEP 517. If you want to force a PEP 517 build, you can use `uv build --force-pep517`.

Thanks @konstin. Is this behavior supposed to be triggered automatically when installing a project with uv declared as the backend? I’m not seeing a uv module in my venv at all. I do have UV_PREVIEW env var set, maybe I also need to use the `--preview` flag? Or perhaps it is the `--no-build-isolation` messing things up here.

`--no-build-isolation` is pretty much a hard requirement on my offline systems that don’t have access to a package index with the build dependencies.

---

_Comment by @konstin on 2025-02-13 18:27_

Can you download the build requirements ahead of time and then use `--find-links <dir>` with them? This should give you a better experience than `--no-build-isolation`, which is mostly there as an escape hatch for packages that don't declare their build dependencies correctly, either because they were written pre-PEP 517 or because they depend on large packages such as torch.

To walk you through the general process, as specified through PEP 517:

When we see a package that we need to build, we create a temp dir. In that temp dir, we run the equivalent of `uv venv` and then `uv pip install ` with the value from `build-system.requires`, plus some dynamic dependencies details I'll skip. After that, we call that Python interpreter, basically `.venv\bin\python -c "import <value of build-system.build-backend>; <value of build-system.build-backend>.build_wheel(<another temp dir>, {})`.

If the build frontend is uv and the build backend is also uv and the versions match, we can skip that: We just call the rust function of the build backend that's conveniently in the same binary. If we do this, we don't need the Python shims. If uv decides that a direct build isn't applicable (which shouldn't be the case with `UV_PREVIEW=1` set, there could be a problem here) or `--force-pep517` is set, we always go the long way, which requires that uv-the-python-package is installed so `import uv; uv.build_wheel(...)` works.

---

_Unassigned @konstin by @konstin on 2025-02-18 20:56_

---

_Closed by @charliermarsh on 2025-03-17 18:13_

---
