```yaml
number: 7749
title: Enable formatting for Jupyter notebooks
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-jupyter
created_at: 2023-10-01T23:34:00Z
updated_at: 2023-10-02T15:02:57Z
url: https://github.com/astral-sh/ruff/pull/7749
synced_at: 2026-01-12T15:55:24Z
```

# Enable formatting for Jupyter notebooks

---

_@charliermarsh_

## Summary

This PR enables `ruff format` to format Jupyter notebooks.

Most of the work is contained in a new `format_source` method that formats a generic `SourceKind`, then returns `Some(transformed)` if the source required formatting, or `None` otherwise.

Closes https://github.com/astral-sh/ruff/issues/7598.

## Test Plan

Ran `cat foo.py | cargo run -p ruff_cli -- format --stdin-filename Untitled.ipynb`; verified that the console showed a reasonable error:

```console
warning: Failed to read notebook Untitled.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: EOF while parsing a value at line 1 column 0
```

Ran `cat Untitled.ipynb | cargo run -p ruff_cli -- format --stdin-filename Untitled.ipynb`; verified that the JSON output contained formatted source code.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-01 23:34_

---

_Review requested from @konstin by @charliermarsh on 2023-10-01 23:34_

---

_Label `formatter` added by @charliermarsh on 2023-10-01 23:34_

---

_@charliermarsh reviewed on 2023-10-01 23:34_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:167 on 2023-10-01 23:34_

The clunkiest thing here is the error handling. Gonna try and simplify it.

---

_@charliermarsh reviewed on 2023-10-02 00:04_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:492 on 2023-10-02 00:04_

I removed this, and moved the logic into `SourceKind`, so that it can be used for formatting too.

---

_Comment by @github-actions[bot] on 2023-10-02 00:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:152 on 2023-10-02 09:01_

Isn't that an error condition?

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:183 on 2023-10-02 09:02_

nit: Move out of the match and clone in the for loop instead

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:200 on 2023-10-02 09:13_

We can avoid the allocation here by only reserving once we see the first formatted cell

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format.rs`:285 on 2023-10-02 09:14_

Could we call-by-value with `SourceKind` instead of call-by-reference or does a caller need this for error handling?

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/format_stdin.rs`:42 on 2023-10-02 09:16_

Does this mean we intentionally always reject jupyter notebooks in stdin?

---

_@konstin approved on 2023-10-02 09:19_

---

_@charliermarsh reviewed on 2023-10-02 14:14_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:152 on 2023-10-02 14:14_

No, it's (e.g.) a non-Python notebook. (This is the same as in the linter.)

---

_@charliermarsh reviewed on 2023-10-02 14:17_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format_stdin.rs`:42 on 2023-10-02 14:17_

No, `SourceType::Python` includes Jupyter notebooks. It's a bit confusing, but the destructured `source_type` here is a `PySourceType`, which can either be Python or Jupyter.

---

_@charliermarsh reviewed on 2023-10-02 14:25_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:285 on 2023-10-02 14:25_

In the `stdin` case, we still need access to the unmodified source so it can't be moved. But I can make this clearer with a custom enum.

---

_Merged by @charliermarsh on 2023-10-02 14:44_

---

_Closed by @charliermarsh on 2023-10-02 14:44_

---

_Branch deleted on 2023-10-02 14:44_

---
