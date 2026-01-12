```yaml
number: 8864
title: Stop at the first resolved parent configuration
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
assignees: []
merged: true
base: main
head: fix-8858
created_at: 2023-11-28T04:02:11Z
updated_at: 2023-11-29T04:28:05Z
url: https://github.com/astral-sh/ruff/pull/8864
synced_at: 2026-01-10T23:40:55Z
```

# Stop at the first resolved parent configuration

---

_Pull request opened by @MichaReiser on 2023-11-28 04:02_

## Summary

This is a potential fix for https://github.com/astral-sh/ruff/issues/8858 but requires some more testing. 
I'm throwing this out here to get some feedback on whether this seems correct. The following is my reasoning, although I'm not very familiar with the code. 

The idea of visiting all ancestor paths is to find a configuration for the current directory (or given path). 
Visiting the ancestors is necessary if the configuration happens to be in a parent directory. For example when running ruff in a sub-directory of the project. 

This PR changes the implementation to stop at the first found configuration instead of visiting the entire project chain. 
One potential issue this could introduce is:

```
root:
  - pyproject.toml (excludes subdir)
  - subdir
    - pyproject.toml
      - sub-sub-dir
        - some-python files 
```

Running ruff in `sub-sub-dir` would lint no files in the old version because the files are excluded by the root pyproject.toml. 
The new version lints the files in the sub-sub-dir because it finds the `subdir/pyproject.toml` as the closest pyproject.toml and it
does not exclude the directoy. 

The new behavior seems more correct to me, but I might be overlooking something.

Fixes #8858

## Test Plan

Added regression test. 


---

_Comment by @MichaReiser on 2023-11-28 04:02_

Current dependencies on/for this PR:
* main
  * **PR #8864** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8864?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8864?utm_source=stack-comment).

---

_Label `cli` added by @MichaReiser on 2023-11-28 04:02_

---

_Comment by @MichaReiser on 2023-11-28 04:02_

This is a potential breaking change

---

_Review requested from @charliermarsh by @MichaReiser on 2023-11-28 04:02_

---

_Comment by @github-actions[bot] on 2023-11-28 04:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-28 06:04_

This looks correct to me.

---

_Merged by @MichaReiser on 2023-11-29 04:21_

---

_Closed by @MichaReiser on 2023-11-29 04:21_

---

_Branch deleted on 2023-11-29 04:21_

---
