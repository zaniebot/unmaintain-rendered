---
number: 15509
title: "Support using `uv sync` in pre-commit.ci"
type: issue
state: closed
author: vivodi
labels:
  - enhancement
assignees: []
created_at: 2025-08-25T05:19:07Z
updated_at: 2025-08-26T06:35:17Z
url: https://github.com/astral-sh/uv/issues/15509
synced_at: 2026-01-10T01:25:56Z
---

# Support using `uv sync` in pre-commit.ci

---

_Issue opened by @vivodi on 2025-08-25 05:19_

### Summary

Support using `uv sync` in pre-commit.ci.

pre-commit.ci has no network access at runtime, so `uv sync` cannot download packages during execution. It can only use the packages specified in `additional_dependencies` in `.pre-commit-config.yaml`.

When the build-backend is `hatchling`, `uv sync` requires `hatchling` to be installed. However, the `hatchling` used by `uv` must reside in the cache directory (e.g., `$HOME/.cache/uv`).
The `hatchling` installed via `pre-commit`’s `additional_dependencies` cannot be found by `uv`.

### Solution
I think the solution is for `uv` to be able to use the `hatchling` installed in `.venv`, so that the `hatchling` from `additional_dependencies` can be used by `uv`.




### Example

```yaml
  - repo: local
    hooks:
      - id: run-uv-sync
        name: Run uv sync
        entry: uv sync
        language: system
        additional_dependencies:
          - uv
          - hatchling
```

---

_Label `enhancement` added by @vivodi on 2025-08-25 05:19_

---

_Comment by @konstin on 2025-08-25 13:14_

You can use the hatchling in the venv with `--no-build-isolation`, but we don't recommend as it removes the advantages of build isolation. uv needs either hatchling in its cache or network access to build packages with the regular build isolation settings, I'm not sure if this is possible with just `additional_dependencies`.

---

_Comment by @vivodi on 2025-08-26 06:35_

> You can use the hatchling in the venv with `--no-build-isolation`, but we don't recommend as it removes the advantages of build isolation. uv needs either hatchling in its cache or network access to build packages with the regular build isolation settings, I'm not sure if this is possible with just `additional_dependencies`.

The method you suggested does solve that problem, but the dependencies installed via `additonal_dependencies` are in `/pc/clone/ZPmcVIdcRg6rN_F4voX-ag/py_env-python3.12/` instead of the `.venv` inside the project directory. This requires running `uv sync --active`, but since `/pc/clone/ZPmcVIdcRg6rN_F4voX-ag/py_env-python3.12/` is read-only (due to pre-commit.ci restrictions), the project itself cannot be installed:

```
error: Failed to install: flexget-3.17.18.dev0-py3-none-any.whl (flexget==3.17.18.dev0 (from file:///code))
  Caused by: failed to create directory `/pc/clone/ZPmcVIdcRg6rN_F4voX-ag/py_env-python3.12/lib/python3.12/site-packages/flexget-3.17.18.dev0.dist-info`: Read-only file system (os error 30)
```

Conclusion: I’ve never seen anyone run `uv sync` inside a pre-commit hook, and after trying it myself I found it far too complicated with too many issues. I eventually gave up on this approach and decided to move the related tasks into GitHub Actions instead.



---

_Closed by @vivodi on 2025-08-26 06:35_

---
