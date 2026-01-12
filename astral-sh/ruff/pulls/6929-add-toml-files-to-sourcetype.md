```yaml
number: 6929
title: "Add TOML files to `SourceType`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/toml
created_at: 2023-08-28T01:57:04Z
updated_at: 2023-08-28T15:02:03Z
url: https://github.com/astral-sh/ruff/pull/6929
synced_at: 2026-01-12T02:45:38Z
```

# Add TOML files to `SourceType`

---

_Pull request opened by @charliermarsh on 2023-08-28 01:57_

## Summary

This PR adds a higher-level enum (`SourceType`) around `PySourceType` to allow us to use the same detection path to handle TOML files. Right now, we have ad hoc `is_pyproject_toml` checks littered around, and some codepaths are omitting that logic altogether (like `add_noqa`). Instead, we should always be required to check the source type and handle TOML files as appropriate.

This PR will also help with our pre-commit capabilities. If we add `toml` to pre-commit (to support `pyproject.toml`), pre-commit will start to pass _other_ files to Ruff (along with `poetry.lock` and `Pipfile` -- see [identify](https://github.com/pre-commit/identify/blob/b59996304fea80025d32abc3ac8f7212a9e7380d/identify/extensions.py#L355)). By detecting those files and handling those cases, we avoid attempting to parse them as Python files, which would lead to pre-commit errors. (We tried to add `toml` to pre-commit here (https://github.com/astral-sh/ruff-pre-commit/pull/44), but had to revert here (https://github.com/astral-sh/ruff-pre-commit/pull/45) as it led to the pre-commit hook attempting to parse `poetry.lock` files as Python files.)


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-28 01:57_

---

_Label `internal` added by @charliermarsh on 2023-08-28 01:57_

---

_@charliermarsh reviewed on 2023-08-28 01:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/path.rs`:4 on 2023-08-28 01:58_

Unused.

---

_@charliermarsh reviewed on 2023-08-28 01:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/lib.rs`:86 on 2023-08-28 01:58_

It's slightly problematic that we assume any other files are Python files, but it's the only way to support custom Python file extensions right now. (This is an unchanged behavior.)

---

_@charliermarsh reviewed on 2023-08-28 02:01_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:559 on 2023-08-28 02:01_

It'd be nice to remove this, but `SourceKind` is in `ruff` and the `try_from_path` method relies on a lot of things that are in `ruff_cli`.

---

_@charliermarsh reviewed on 2023-08-28 02:02_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:443 on 2023-08-28 02:02_

This is the kind of thing I'm trying to prevent with this PR. Previously, this code ignored `pyproject.toml` entirely and would've ended up trying to parse `pyproject.toml` files as Python files... Now, we're at least forced to ignore them.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:254 on 2023-08-28 02:03_

Ignore other TOML files.

---

_@charliermarsh reviewed on 2023-08-28 02:03_

---

_@MichaReiser approved on 2023-08-28 07:25_

This seems to be an improvement overall but I do find it confusing that `SourceType` now mixes identifying dedicated file types / dialects AND identifying files by name. 

My longterm goal to untangle this would be to move the whole Toml handling out of `ruff` (or at least, `ruff_python_linter`) by:

* Introducing a new `ExtensionHandler` trait similar to rome https://github.com/rome/tools/blob/1dcd62b169e875b7048f63efb5f6636d2152334c/crates/rome_service/src/file_handlers/mod.rs#L249-L273
* Moving the toml linting to its own crate with its own `ExtensionHandler`. 
* The CLI uses the extension handlers to find the right handler, then calls `lint` if the handler supports linting. 



---

_Comment by @charliermarsh on 2023-08-28 14:35_

I feel similarly, which is that this is an improvement but the abstractions are lacking.

---

_Merged by @charliermarsh on 2023-08-28 15:01_

---

_Closed by @charliermarsh on 2023-08-28 15:01_

---

_Branch deleted on 2023-08-28 15:01_

---

_Comment by @github-actions[bot] on 2023-08-28 15:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
