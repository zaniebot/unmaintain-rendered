```yaml
number: 7241
title: inconsistent behaviour of uv export with --no-emit-workspace option for the root project
type: issue
state: closed
author: lucidfrontier45
labels:
  - bug
assignees: []
created_at: 2024-09-10T02:54:20Z
updated_at: 2024-09-10T13:27:46Z
url: https://github.com/astral-sh/uv/issues/7241
synced_at: 2026-01-12T15:59:11Z
```

# inconsistent behaviour of uv export with --no-emit-workspace option for the root project

---

_@lucidfrontier45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

- uv platform: linux gnu
- uv version: 0.4.8
- uv command: `uv export --no-emit-workspace`


https://docs.astral.sh/uv/reference/cli/#uv-export
```
--no-emit-workspace
Do not emit any workspace members, including the root project.

By default, all workspace members and their dependencies are included in the exported requirements file, with all of their dependencies. The --no-emit-workspace option allows exclusion of all the workspace members while retaining their dependencies.
```

From this documentation I expected that passing `--no-emit-workspace` will also prevent emitting `-e .` to the requirements.txt. For a project with sub modules I confirmed that 

```
-e .
-e sub1
-e sub2
...
```
disappeared.

However if I use this option to a project that has no sub modules, i.e. no entries of `tool.uv.workspace.members`, `-e .` is still emitted. Isn't `-e .` supposed to be removed in this case?

---

_Comment by @charliermarsh on 2024-09-10 03:27_

Yes. Can you provide a full reproduction though? I.e., a file that I can use to reproduce this, or a series of commands to run?

---

_Comment by @lucidfrontier45 on 2024-09-10 04:25_

@charliermarsh 

Thank you for reply. Just type those commands and you can see `-e .` is included in the exported output.

```sh
uv init --lib new_project
cd new_project
uv export --no-emit-workspace
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-10 11:57_

---

_Label `bug` added by @charliermarsh on 2024-09-10 11:57_

---

_Closed by @charliermarsh on 2024-09-10 13:27_

---
