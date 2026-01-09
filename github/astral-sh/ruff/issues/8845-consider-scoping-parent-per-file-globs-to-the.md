---
number: 8845
title: Consider scoping parent per-file globs to the base project when 
type: issue
state: closed
author: strangemonad
labels: []
assignees: []
created_at: 2023-11-26T21:37:05Z
updated_at: 2023-11-27T15:53:18Z
url: https://github.com/astral-sh/ruff/issues/8845
synced_at: 2026-01-07T13:12:15-06:00
---

# Consider scoping parent per-file globs to the base project when 

---

_Issue opened by @strangemonad on 2023-11-26 21:37_

Summary: per-file-ignores globs in a parent config file seem to resolve relative to the parent's location rather than the base project.

Consider the following
```
project_a/
|_ pyproject.toml (extends ../config/shared-ruff.toml)
|_ src/...
|_ tests/...

config/
|_ shared-ruff.toml (attempts to set a per-file-ignores glob e.g. to exclude D100 from any tests/**/*.py file)
```

(Note that the parent config dirname isn't strictly a prefix of the other projects).

Expected: Sharing settings that involve globs across projects with similar layouts should be possible somehow. This seems in line with the other settings when extending config files e.g. https://docs.astral.sh/ruff/settings/#src

Actual:
The globs seems to be resolved as absolute paths in `/config/tests/**/*.py`. When the workspace is resolved, each configuration is given its own `project_root` with the path to the current config file being parsed. This means only the base-name glob matcher can be used e.g. I can exclude files like `__init__.py` or `*.py` but not `tests/__init__.py` or more complex globs.


Workaround:
It's possible to move the config file to a shared parent folder and use a prefix glob e.g. `**/tests/*.py`

---

_Comment by @zanieb on 2023-11-27 15:53_

Thanks for the report. This looks like a duplicate of https://github.com/astral-sh/ruff/issues/8795, we can track this there.

---

_Closed by @zanieb on 2023-11-27 15:53_

---
