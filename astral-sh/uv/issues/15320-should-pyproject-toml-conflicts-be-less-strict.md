```yaml
number: 15320
title: Should pyproject.toml conflicts be less strict?
type: issue
state: closed
author: alexrockhill
labels:
  - question
assignees: []
created_at: 2025-08-15T22:57:15Z
updated_at: 2025-08-15T23:23:17Z
url: https://github.com/astral-sh/uv/issues/15320
synced_at: 2026-01-12T16:02:08Z
```

# Should pyproject.toml conflicts be less strict?

---

_@alexrockhill_

### Question

```
git clone https://github.com/braindecode/braindecode.git
cd braindecode
uv sync
```

Causes

```
braindecode$ uv sync
Using CPython 3.13.3
Removed virtual environment at: .venv
Creating virtual environment at: .venv
  × No solution found when resolving dependencies for split (python_full_version > '3.9' and python_full_version
  │ < '3.10'):
  ╰─▶ Because the requested Python version (>3.9) does not satisfy Python>=3.10 and mne>=1.10.0 depends on
      Python>=3.10, we can conclude that mne>=1.10.0 cannot be used.
      And because only the following versions of mne are available:
          mne<=1.10.0
          mne==1.10.1
      we can conclude that mne>=1.10.0 cannot be used.
      And because your project depends on mne>=1.10.0 and your project requires braindecode[docs], we can conclude
      that your project's requirements are unsatisfiable.

      hint: The `requires-python` value (>3.9) includes Python versions that are not supported by your dependencies
      (e.g., mne>=1.10.0 only supports >=3.10). Consider using a more restrictive `requires-python` value (like
      >=3.10).
```

as described here: https://github.com/braindecode/braindecode/issues/763

Since I have 3.13 pinned or any other version >= 3.10. It seems unexpected to me that `uv sync` would fail instead of just realizing that although the minimum version requirements are incompatible, they are not applicable. Clearly this is most relevant for using `uv` in packages not managed by `uv` by their maintainers but this seems like maybe a simple bug fix?

If you just edit the `pyproject.toml` file to say `requires-python>=3.10` then it syncs just fine also important to mention (the dependency `mne` requires >= 3.10).

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @alexrockhill on 2025-08-15 22:57_

---

_Comment by @zanieb on 2025-08-15 22:59_

See https://docs.astral.sh/uv/concepts/resolution/#universal-resolution

The failure is in constructing a lockfile that is valid for the project, not in performing a sync for 3.13 specifically.

---

_Comment by @alexrockhill on 2025-08-15 23:23_

Thanks! Maybe the question is then should the uv.lock file just handle that and override the pyproject.toml? Probably not and you leave that to the library to handle as I was asking about/suggesting in the linked issue. That answers my question but I think from a user experience, not ideal, so maybe something to consider, I'm not sure.

---

_Closed by @alexrockhill on 2025-08-15 23:23_

---
