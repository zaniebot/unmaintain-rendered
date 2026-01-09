---
number: 16610
title: Init a project with a different module name
type: issue
state: closed
author: vepain
labels:
  - enhancement
assignees: []
created_at: 2025-11-06T10:46:32Z
updated_at: 2025-12-19T13:13:06Z
url: https://github.com/astral-sh/uv/issues/16610
synced_at: 2026-01-07T13:12:19-06:00
---

# Init a project with a different module name

---

_Issue opened by @vepain on 2025-11-06 10:46_

### Summary

`uv init` has the option `--name` to give a project name that is different from the project directory name.

It could be great to also have an option `--module-name` to have an import name that is different from the project name.

### Example

Scikit-learn project is named as it, while we can import it as `sklearn` (shorter name).

Suppose I want to create a project name `project-name`, in a directory `project_name-py` I can import thanks to `import proj`. A `uv init` command could be `uv init --name project-name --module-name proj project_name-py`

---

_Label `enhancement` added by @vepain on 2025-11-06 10:46_

---

_Comment by @vepain on 2025-11-06 10:57_

According to https://docs.astral.sh/uv/concepts/build-backend/#modules, an option like `--module-name` implies:

1. Precising the path of the module:

```toml
[tool.uv.build-backend]
    module-name = "proj"
```

2. In the case of the option `--package`, use the correct path for script:

```toml
[project.scripts]
    proj = "proj:main"
```

---

_Comment by @konstin on 2025-12-18 15:22_

We generally recommend use the same module name as package name, it's much easier to understand for users and it avoids conflicts with other packages using the same module name, so while it's possible to have different names and we support that, I don't think we want to encourage it in `uv init`.

---

_Comment by @vepain on 2025-12-19 13:13_

Understood, I close it.

---

_Closed by @vepain on 2025-12-19 13:13_

---
