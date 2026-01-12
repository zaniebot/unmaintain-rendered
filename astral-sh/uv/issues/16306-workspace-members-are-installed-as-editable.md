```yaml
number: 16306
title: "Workspace members are installed as editable during `uv tool install ./project`"
type: issue
state: open
author: krishmas
labels:
  - bug
assignees: []
created_at: 2025-10-15T01:53:31Z
updated_at: 2025-10-17T08:36:04Z
url: https://github.com/astral-sh/uv/issues/16306
synced_at: 2026-01-12T16:02:28Z
```

# Workspace members are installed as editable during `uv tool install ./project`

---

_@krishmas_

### Question

I raised this as a related question in #12795 but figured this is a little different and deserved its own issue

  I have a monorepo workspace structure with multiple projects that depend on shared libraries within the same workspace:
```
  monorepo/
  ├── pyproject.toml (workspace root)
  ├── projects/
  │   └── project_a/
  │       ├── pyproject.toml
  │       └── src/project_a/
  └── shared_libs/
      └── shared_lib_a/
          ├── pyproject.toml
          └── src/shared_lib_a/
```

  **Root `pyproject.toml`:**
  ```toml
  [tool.uv.workspace]
  members = [
      "projects/project_a",
      "shared_libs/shared_lib_a"
  ]

  [tool.uv.sources]
  project_a = { workspace = true }
  shared_lib_a = { workspace = true }
```

  `projects/project_a/pyproject.toml:`
```toml
  [project]
  name = "project_a"
  dependencies = [
      "shared_lib_a"
  ]

  [project.scripts]
  project_a = "project_a.main:app"

  [build-system]
  requires = ["hatchling"]
  build-backend = "hatchling.build"
```

  When I run `uv tool install project_a`, the installation creates a snapshot but only includes a .pth file linking back to `shared_lib_a`'s source code. This means if I modify or delete `shared_lib_a`
  after installation, the installed tool breaks.

  Is there a way to make `uv tool install` include/vendor the workspace dependencies (`shared_lib_a`) into the tool installation so it's fully self-contained?

  I basically want the installed tool to be independent of the workspace source code state, similar to how compiled binaries work in Go/Rust/etc where dependencies are bundled into the final artifact.

### Platform

_No response_

### Version

uv 0.7.8 (0ddcc1905 2025-05-23)

---

_Label `question` added by @krishmas on 2025-10-15 01:53_

---

_Comment by @zanieb on 2025-10-15 13:49_

I could reproduce this with

```
❯ uv init --package example
Initialized project `example` at `/private/tmp/example`
❯ cd example
❯ uv init child
Adding `child` as member of workspace `/private/tmp/example`
Initialized project `child` at `/private/tmp/example/child`
❯ uv add child
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 2 packages in 24ms
      Built example @ file:///private/tmp/example
      Built child @ file:///private/tmp/example/child
Prepared 2 packages in 534ms
Installed 2 packages in 3ms
 + child==0.1.0 (from file:///private/tmp/example/child)
 + example==0.1.0 (from file:///private/tmp/example)
❯ cd ..

❯ uv tool install ./example
Resolved 2 packages in 1ms
      Built example @ file:///private/tmp/example
Prepared 1 package in 5ms
Installed 2 packages in 2ms
 + child==0.1.0 (from file:///private/tmp/example/child)
 + example==0.1.0 (from file:///private/tmp/example)
Installed 1 executable: example
                                                                                              
❯ ls $(uv_tool_dir)/example/lib/python3.14/site-packages/
__editable__.child-0.1.0.pth        child-0.1.0.dist-info/
__editable___child_0_1_0_finder.py  example/
_virtualenv.pth                     example-0.1.0.dist-info/
_virtualenv.py          
```

I'd expect us to use non-editable installs here by default. I think that's either a bug, or we should expose `--no-editable` to opt-out.

---

_Label `question` removed by @zanieb on 2025-10-15 13:49_

---

_Label `bug` added by @zanieb on 2025-10-15 13:49_

---

_Renamed from "Can `uv tool install` include workspace dependencies in the installation?" to "Workspace members are installed as editable during `uv tool install ./project`" by @zanieb on 2025-10-15 13:50_

---

_Comment by @krishmas on 2025-10-17 08:28_

Hi @zanieb ! I took a stab at this with [#16335](https://github.com/astral-sh/uv/pull/16335), apologies for starting work on an issue that wasn't labeled as appropriate for community contribution. Definitely agree with that perspective after reading the contribution guidelines. In my case I got too deep by the time I read that and figured it would at least be worth putting up a PR since (at least IMO) the solution in the PR seems fairly uncontroversial and solved my issues with `uv tool install`.

For this impl, rather than adding a `--no-editable` flag, I agree with you that that should just be the default behavior for tool installs, especially since `uv tool install` already supports an `--editable` flag.

Would appreciate a review if possible (some optional CI checks are failing due to 403 but besides that they're all passing), but definitely understand if you'd rather this be handled internally due to it not being explicitly tagged as `help wanted`

---
