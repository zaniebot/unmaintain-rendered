```yaml
number: 15312
title: "Add `uv tool list --show-python` to show python version"
type: pull_request
state: closed
author: pythonweb2
labels: []
assignees: []
base: main
head: uv-tool-add-python-list-option
created_at: 2025-08-15T15:52:53Z
updated_at: 2025-10-10T17:43:23Z
url: https://github.com/astral-sh/uv/pull/15312
synced_at: 2026-01-10T06:36:15Z
```

# Add `uv tool list --show-python` to show python version

---

_Pull request opened by @pythonweb2 on 2025-08-15 15:52_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Display information about the python version when using the `uv tool list` cli command.

## Test Plan

Added a test that tests the option, and then also a test that tests all of the `--show-*` options in the command.


---

_@pythonweb2 reviewed on 2025-08-15 15:54_

---

_Review comment by @pythonweb2 on `crates/uv-cli/src/lib.rs`:4584 on 2025-08-15 15:54_

```suggestion
    /// Whether to display the Python installation used to run each tool.
```

---

_Comment by @zanieb on 2025-08-15 18:58_

We'll need a test case in `it/tool_list` too

---

_Comment by @pythonweb2 on 2025-08-15 21:01_

I added tests @zanieb and made some changes in where the version gets pulled from. Lmk if it looks OK.

---

_@zanieb reviewed on 2025-08-15 21:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:102 on 2025-08-15 21:34_

Hm. I'm surprised we don't need to read the environment elsewhere here.

`installed_tools.version(&name, cache)` reads the environment. We probably don't want to do that multiple times. 
I think might want to get the environment above and re-use it for both of these operations. That'll be more performant and we don't need to handle error cases multiple times. 

I'd probably do something like... 

- Add a new `ToolEnvironment` type that's returned from `get_environment`. It can just wrap `PythonEnvironment`. You can implement deref, `into_inner`, etc. (see other patterns like that) so it's not annoying to work with. There are only a few callsites so this shouldn't be bad.
- Move the `InstalledTools::version` function to `ToolEnvironment`. There are also only a couple call-sites to update here too.
- Now in this function, get the environment once, then derive the tool and interpreter versions from it.



---

_@zanieb reviewed on 2025-08-15 21:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-08-15 21:36_

I'd just use `interpreter.python_full_version()`. I think we also probably want to include the implementation name. e.g., `CPython 3.13.1`.

---

_@zanieb reviewed on 2025-08-15 21:36_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:107 on 2025-08-15 21:36_

We should skip display of the entire tool if the environment is missing. That should be resolve by the refactor I suggested.

---

_@zanieb reviewed on 2025-08-15 21:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:108 on 2025-08-15 21:37_

We'll want to display this as a warning as done for tool version handling — this should also be resolved by the refactor.

---

_@zanieb reviewed on 2025-08-15 21:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:105 on 2025-08-15 21:37_

Alternatively we could display `[python: <version>]`. I don't know if I feel strongly. That'd be consistent with the other displays? but `[python: <implementation> <version>]` feels too verbose.

---

_Review comment by @pythonweb2 on `crates/uv/src/commands/tool/list.rs`:105 on 2025-08-15 21:46_

Yeah I thought that `[python: ...` felt verbose as well. If the implementation is added, is seems like that would be self-explanatory enough.

---

_@pythonweb2 reviewed on 2025-08-15 21:46_

---

_Closed by @zanieb on 2025-10-10 17:33_

---

_Branch deleted on 2025-10-10 17:43_

---
