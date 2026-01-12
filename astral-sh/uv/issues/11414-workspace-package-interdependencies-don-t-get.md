```yaml
number: 11414
title: "Workspace package interdependencies don't get bundled in wheels/sdist"
type: issue
state: open
author: yashgorana
labels:
  - question
assignees: []
created_at: 2025-02-11T05:40:07Z
updated_at: 2025-02-11T08:40:07Z
url: https://github.com/astral-sh/uv/issues/11414
synced_at: 2026-01-12T16:00:36Z
```

# Workspace package interdependencies don't get bundled in wheels/sdist

---

_@yashgorana_

### Summary

In a UV workspace, packages that have inter-dependencies to other workspace packages, don't get bundled in the final .whl file.

Workspace setup: https://github.com/OpenMined/syft-extras/

For example, syft-rpc depends on syft-core, but when we run the following, 
```
uv build --package syft-rpc

python -m venv .venv
source .venv/bin/activate
pip install ./dist/syft_rpc-0.1.0-py3-none-any.whl
```

fails with an error
```
Collecting pydantic>=2.9.2 (from syft-rpc==0.1.0)
  Using cached pydantic-2.10.6-py3-none-any.whl.metadata (30 kB)
INFO: pip is looking at multiple versions of syft-rpc to determine which version is compatible with other requirements. This could take a while.
ERROR: Could not find a version that satisfies the requirement syft-core (from syft-rpc) (from versions: none)

ERROR: No matching distribution found for syft-core
```

### Platform

Darwin 23.6.0 arm64

### Version

0.5.29

### Python version

3.12.9

---

_Label `bug` added by @yashgorana on 2025-02-11 05:40_

---

_Comment by @yashgorana on 2025-02-11 05:58_

headsup - we just pushed individual packages to pypi
https://pypi.org/project/syft-core/
https://pypi.org/project/syft-event/
https://pypi.org/project/syft-rpc/

and now it works - but ideally i'd want to not have them uploaded individually?

---

_Comment by @konstin on 2025-02-11 08:40_

The idea of a workspace is that each package still has its own `pyproject.toml` with its own metadata and dependencies. Each of them is packaged in its own wheel with a single root module to enforce the isolation of the dependency specification.

Python does not have a concept of application bundles beyond an installed venvs, so we have to write multiple files for a single workspace.

You should be able to upload all of them with a single `uv publish` command though.

---

_Label `bug` removed by @konstin on 2025-02-11 08:40_

---

_Label `question` added by @konstin on 2025-02-11 08:40_

---
