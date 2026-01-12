```yaml
number: 8818
title: "False positive in PYI files with ellipsis for `PIE796` "
type: issue
state: closed
author: Skylion007
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-11-22T15:30:37Z
updated_at: 2023-11-24T15:24:59Z
url: https://github.com/astral-sh/ruff/issues/8818
synced_at: 2026-01-12T15:54:48Z
```

# False positive in PYI files with ellipsis for `PIE796` 

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
If I define an enum file with ellipsis in PYI files, the linter complains
```
class CreationMeta(Enum):
    DEFAULT = ...
    IN_CUSTOM_FUNCTION = ...
    MULTI_OUTPUT_NODE = ...
    NO_GRAD_MODE = ...
    INFERENCE_MODE = ...
```

in a .PYI files, these values are all set to `...` to denote that they will be populated in other code, and we only care about what the members of the enum are. This is a pretty common idiom so we may want to explicitly whitelist in our check or at least propose a better fix for PYI files as I am unaware of another way of doing this with a placeholder value for `Enum`.



---

_Label `bug` added by @zanieb on 2023-11-22 16:10_

---

_Comment by @charliermarsh on 2023-11-22 17:03_

Agree, this makes sense to fix IMO.

---

_Label `good first issue` added by @zanieb on 2023-11-22 17:54_

---

_Comment by @Avasam on 2023-11-22 17:54_

In a strict statically type-checked context, I feel this might still be undesirable? But that's more for a rule like https://github.com/PyCQA/flake8-pyi/issues/98

---

_Closed by @charliermarsh on 2023-11-24 15:24_

---
