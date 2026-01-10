```yaml
number: 7731
title: "Depedency with the same name as the project doesn't error"
type: issue
state: closed
author: 2531
labels:
  - bug
assignees: []
created_at: 2024-09-27T06:16:59Z
updated_at: 2024-09-28T20:02:12Z
url: https://github.com/astral-sh/uv/issues/7731
synced_at: 2026-01-10T04:45:10Z
```

# Depedency with the same name as the project doesn't error

---

_Issue opened by @2531 on 2024-09-27 06:16_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
If you add a dependency with the same name as the project name, nothing happens. Although this may seem strange, project names are free!
The steps are as follows
```bash
uv init ruff
cd ruff
uv add ruff
```


---

_Renamed from "Add dependency issue" to "Depedency with the same name as the project doesn't error" by @konstin on 2024-09-27 09:53_

---

_Label `bug` added by @konstin on 2024-09-27 09:53_

---

_Comment by @charliermarsh on 2024-09-27 12:20_

I guess we should have a dedicated error here? In some strange sense this is correct though... Projects do sometimes have circular dependencies.

```toml
[project]
name = "ruff"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "ruff",
]

[tool.uv.sources]
ruff = { workspace = true }
```

---

_Comment by @2531 on 2024-09-27 16:36_

Yes, after further testing I realized that the dependencies are also not actually added to the environment when the project name is the same as the implicit dependency name. Often we initialize the project before adding the dependencies and it is also unlikely that we are aware of all the implicit dependency names, and when adding the main dependency package, implicit dependencies that have the same name as the project name are not added, which can cause unexpected problems.


---

_Comment by @notatallshaw on 2024-09-27 17:14_

One important use case for circular dependencies is extras, you might want your `all` extra to depend on your `test` and `dev` extra and you would define it as `all = ["mypkg[test]", "mypkg[dev]"]`.

But it does seem odd for a project to depend on itself with no additional conditions, if you're already installing the project you don't need to specify you need to install your project for your project to work.

---

_Comment by @luaa9 on 2024-09-28 16:22_

I ran into a possibly related issue where the directory name is the same as the python dependency.

For example:

```
uv init lark
cd lark
uv add lark
uv run <Python script using lark>
```
The script won't be able to find the module lark

It's understandable if this might be a restriction, but perhaps a note should be  added to the documentation?.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-28 19:42_

---

_Closed by @charliermarsh on 2024-09-28 20:02_

---

_Closed by @charliermarsh on 2024-09-28 20:02_

---
