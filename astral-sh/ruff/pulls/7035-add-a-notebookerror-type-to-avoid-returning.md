```yaml
number: 7035
title: "Add a `NotebookError` type to avoid returning `Diagnostics` on error"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/box-error
created_at: 2023-08-31T22:17:03Z
updated_at: 2023-09-13T09:46:07Z
url: https://github.com/astral-sh/ruff/pull/7035
synced_at: 2026-01-12T02:45:38Z
```

# Add a `NotebookError` type to avoid returning `Diagnostics` on error

---

_Pull request opened by @charliermarsh on 2023-08-31 22:17_

## Summary

This PR refactors the error-handling cases around Jupyter notebooks to use errors rather than `Box<Diagnostics>`, which creates some oddities in the downstream handling. So, instead of formatting errors as diagnostics _eagerly_ (in the notebook methods), we now return errors and convert those errors to diagnostics at the last possible moment (in `diagnostics.rs`). This is more ergonomic, as errors can be composed and reported-on in different ways, whereas diagnostics require a `Printer`, etc.

See, e.g., https://github.com/astral-sh/ruff/pull/7013#discussion_r1311136301.

## Test Plan

Ran `cargo run` over a Python file labeled with a `.ipynb` suffix, and saw:

```
foo.ipynb:1:1: E999 SyntaxError: Expected a Jupyter Notebook, which must be internally stored as JSON, but found a Python source file: expected value at line 1 column 1
```


---

_Marked ready for review by @charliermarsh on 2023-08-31 22:17_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-31 22:17_

---

_Label `internal` added by @charliermarsh on 2023-08-31 22:17_

---

_@charliermarsh reviewed on 2023-08-31 22:17_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:140 on 2023-08-31 22:17_

Return `Result<Option<Notebook>>` instead of using empty `Diagnostics` to indicate the "we parsed the notebook, but it's not a Python notebook, so it should be skipped" case.

---

_Comment by @charliermarsh on 2023-08-31 22:19_

The `jupyter` module no longer depends on much in the `ruff` crate -- just the source map stuff in `autofix`. This PR removes the dependency on diagnostics, specific rules, etc. So we could move it into its own crate, if we want.

---

_Comment by @github-actions[bot] on 2023-08-31 22:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/notebook.rs`:162 on 2023-09-01 04:12_

nit: we can remove this comment now that the error isn't being converted to a diagnostic

---

_@dhruvmanila approved on 2023-09-01 04:20_

Thanks a ton! This is much better :)

---

_Comment by @dhruvmanila on 2023-09-01 04:21_

> So we could move it into its own crate, if we want.

Yeah, that's nice. I think once we establish some abstraction around linting multiple filetypes it would be good to move this into it's own crate (`ruff_notebook`, `ruff_ipython_notebook`?)

---

_Comment by @dhruvmanila on 2023-09-01 05:05_

Current dependencies on/for this PR:
* main
  * **PR #7035** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7035" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7013** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7013" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #6659** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/6659" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #7041** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7041" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #7211** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7211" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #7263** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7263" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #7331** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7331" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                  * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                      * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
                        * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7035?utm_source=stack-comment).

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:140 on 2023-09-01 06:20_

Should this function (and `notebook_from_source_code`) be methods on `Notebook` instead.

```
Notebook::from_path(path)?.as_python_notebook()
Notebook::from_source_code(code)?.as_python_notebook()
```

What I don't know is where to move  the debug logging, assuming is necessary.

---

_@MichaReiser approved on 2023-09-01 06:20_

---

_@charliermarsh reviewed on 2023-09-01 10:13_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:140 on 2023-09-01 10:13_

Yeah they should. I bet this was hard before when it relied on `Diagnostics`.

---

_Merged by @charliermarsh on 2023-09-01 11:08_

---

_Closed by @charliermarsh on 2023-09-01 11:08_

---

_Branch deleted on 2023-09-01 11:08_

---
