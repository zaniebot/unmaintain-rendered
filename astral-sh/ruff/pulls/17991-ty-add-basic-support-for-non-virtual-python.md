```yaml
number: 17991
title: "[ty] Add basic support for non-virtual Python environments"
type: pull_request
state: merged
author: zanieb
labels:
  - ty
assignees: []
merged: true
base: main
head: zb/python-env
created_at: 2025-05-09T17:28:24Z
updated_at: 2025-05-10T20:20:02Z
url: https://github.com/astral-sh/ruff/pull/17991
synced_at: 2026-01-12T15:56:09Z
```

# [ty] Add basic support for non-virtual Python environments

---

_@zanieb_

This adds basic support for non-virtual Python environments by accepting a directory without a `pyvenv.cfg` which allows existing, subsequent site-packages discovery logic to succeed. We can do better here in the long-term, by adding more eager validation (for error messages) and parsing the Python version from the discovered site-packages directory (which isn't relevant yet, because we don't use the discovered Python version from virtual environments as the default `--python-version` yet either). 

Related

- https://github.com/astral-sh/ty/issues/265
- https://github.com/astral-sh/ty/issues/193

You can review this commit by commit if it makes you happy.

I tested this manually; I think refactoring the test setup is going to be a bit more invasive so I'll stack it on top (see https://github.com/astral-sh/ruff/pull/17996).

```
❯ uv run ty check --python /Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none/ -vv example
2025-05-09 12:06:33.685911 DEBUG Version: 0.0.0-alpha.7 (f9c4c8999 2025-05-08)
2025-05-09 12:06:33.685987 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-05-09 12:06:33.686002 DEBUG Searching for a project in '/Users/zb/workspace/ty'
2025-05-09 12:06:33.686123 DEBUG Resolving requires-python constraint: `>=3.8`
2025-05-09 12:06:33.686129 DEBUG Resolved requires-python constraint to: 3.8
2025-05-09 12:06:33.686142 DEBUG Project without `tool.ty` section: '/Users/zb/workspace/ty'
2025-05-09 12:06:33.686147 DEBUG Searching for a user-level configuration at `/Users/zb/.config/ty/ty.toml`
2025-05-09 12:06:33.686156 INFO Defaulting to python-platform `darwin`
2025-05-09 12:06:33.68636 INFO Python version: Python 3.8, platform: darwin
2025-05-09 12:06:33.686375 DEBUG Adding first-party search path '/Users/zb/workspace/ty'
2025-05-09 12:06:33.68638 DEBUG Using vendored stdlib
2025-05-09 12:06:33.686634 DEBUG Discovering site-packages paths from sys-prefix `/Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none` (`--python` argument')
2025-05-09 12:06:33.686667 DEBUG Attempting to parse virtual environment metadata at '/Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none/pyvenv.cfg'
2025-05-09 12:06:33.686671 DEBUG Searching for site-packages directory in `sys.prefix` path `/Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none`
2025-05-09 12:06:33.686702 DEBUG Resolved site-packages directories for this environment are: ["/Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none/lib/python3.10/site-packages"]
2025-05-09 12:06:33.686706 DEBUG Adding site-packages search path '/Users/zb/.local/share/uv/python/cpython-3.10.17-macos-aarch64-none/lib/python3.10/site-packages'
...

❯ uv run ty check --python /tmp -vv example
2025-05-09 15:36:10.819416 DEBUG Version: 0.0.0-alpha.7 (f9c4c8999 2025-05-08)
2025-05-09 15:36:10.819708 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-05-09 15:36:10.820118 DEBUG Searching for a project in '/Users/zb/workspace/ty'
2025-05-09 15:36:10.821652 DEBUG Resolving requires-python constraint: `>=3.8`
2025-05-09 15:36:10.821667 DEBUG Resolved requires-python constraint to: 3.8
2025-05-09 15:36:10.8217 DEBUG Project without `tool.ty` section: '/Users/zb/workspace/ty'
2025-05-09 15:36:10.821888 DEBUG Searching for a user-level configuration at `/Users/zb/.config/ty/ty.toml`
2025-05-09 15:36:10.822072 INFO Defaulting to python-platform `darwin`
2025-05-09 15:36:10.822439 INFO Python version: Python 3.8, platform: darwin
2025-05-09 15:36:10.822773 DEBUG Adding first-party search path '/Users/zb/workspace/ty'
2025-05-09 15:36:10.822929 DEBUG Using vendored stdlib
2025-05-09 15:36:10.829872 DEBUG Discovering site-packages paths from sys-prefix `/tmp` (`--python` argument')
2025-05-09 15:36:10.829911 DEBUG Attempting to parse virtual environment metadata at '/private/tmp/pyvenv.cfg'
2025-05-09 15:36:10.829917 DEBUG Searching for site-packages directory in `sys.prefix` path `/private/tmp`
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: Failed to search the `lib` directory of the Python installation at `sys.prefix` path `/private/tmp` for `site-packages`
```

---

_Label `ty` added by @zanieb on 2025-05-09 17:28_

---

_Comment by @github-actions[bot] on 2025-05-09 17:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`

```
</details>


---

_Marked ready for review by @zanieb on 2025-05-09 21:10_

---

_Review requested from @carljm by @zanieb on 2025-05-09 21:10_

---

_Review requested from @AlexWaygood by @zanieb on 2025-05-09 21:10_

---

_Review requested from @sharkdp by @zanieb on 2025-05-09 21:10_

---

_Review requested from @dcreager by @zanieb on 2025-05-09 21:10_

---

_@zanieb reviewed on 2025-05-09 22:11_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:44 on 2025-05-09 22:11_

We should probably not use this fallback if the `origin` is specific to virtual environments, I can do that in a follow-up since it's not critical and I'll need to build on #17996 to add test coverage.

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:44 on 2025-05-10 12:52_

See https://github.com/astral-sh/ruff/pull/18003

---

_@zanieb reviewed on 2025-05-10 12:52_

---

_Renamed from "Add basic support for non-virtual Python environments" to "[ty] Add basic support for non-virtual Python environments" by @zanieb on 2025-05-10 12:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:617 on 2025-05-10 14:05_

this is part of the `prelude`

```suggestion
```

---

_@AlexWaygood approved on 2025-05-10 14:09_

Thank you!

---

_@zanieb reviewed on 2025-05-10 14:39_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:617 on 2025-05-10 14:39_

No clue why the editor added that / why Clippy wouldn't remove it?

---

_@zanieb reviewed on 2025-05-10 14:46_

---

_Review comment by @zanieb on `crates/ty_python_semantic/src/site_packages.rs`:617 on 2025-05-10 14:46_

(I'll remove this before merging, but won't update it until then because I'll need to rebase the whole stack)

---

_Merged by @zanieb on 2025-05-10 20:17_

---

_Closed by @zanieb on 2025-05-10 20:17_

---

_Branch deleted on 2025-05-10 20:17_

---
