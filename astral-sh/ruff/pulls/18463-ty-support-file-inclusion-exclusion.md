```yaml
number: 18463
title: "[ty] Support file inclusion/exclusion"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
draft: true
base: main
head: micha/file-inclusion-exclusion
created_at: 2025-06-04T15:13:50Z
updated_at: 2025-06-06T13:24:06Z
url: https://github.com/astral-sh/ruff/pull/18463
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Support file inclusion/exclusion

---

_Pull request opened by @MichaReiser on 2025-06-04 15:13_

## Summary

In progress. 

This adds a new `src.files` configuration option. 

`src.files` supports positive and negative glob patterns:

* `!.git`: Ignores everything in the `./.git` directory
* `src/**`: Includes everything in the `./src` directory 

There are a few differences to git ignore:

1. All paths are anchored. `src` in gitignore matches `src` at any level, e.g. it matches `./src` but also `tests/src`. Anchoring the paths allows us to skip paths that are known not to be included and I think it is also less surprising (but the behavior does make sense for gitignore)
2. ty uses a heuristics to detect directory inclusions that aren't globs. E.g. `src` will be turned into `src/**` to include `src` and all files. I think this makes sense because `!src` excludes the entire folder and its contents. It's also the first thing I would try. Writing `src/**` seems hard to remember. 

TODO:

* [ ] Error handling
* [ ] `--include` and `--exclude` CLI options
* [ ] Tests, a lot of them
* [ ] Restrict the glob syntax to what uv permits
* [ ] Docs
* [ ] Filtering when calling `is_included` outside of directory traversal
* [ ] Default exclusion: This should be as simple as defining a list that's inserted first into `files`



Part of https://github.com/astral-sh/ty/issues/176




## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @MichaReiser on 2025-06-04 15:13_

---

_Comment by @github-actions[bot] on 2025-06-04 15:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- WARN No python files found under the given path(s)
+ warning[no-files] No python files found under the given path(s)
- Found 1 diagnostic
+ Found 2 diagnostics

```
</details>


---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:473 on 2025-06-05 06:05_

TODO: This is wrong. We need to track for each segment if it is dynamic or not and if we add a dynamic segment, prune all longer segments.

---

_@MichaReiser reviewed on 2025-06-05 06:05_

---

_Review comment by @MichaReiser on `crates/ty_project/src/walk.rs`:427 on 2025-06-05 10:17_

I think we also need to do this if the pattern's negated or the following won't work:

```
files = ["src", "!src"]
```

There's no issue when we traverse the directory top down, but there's an issue if we match an individual file, e.g `src/a.py`, in which case `src/**` would still match, but `!src` doesn't.

---

_@MichaReiser reviewed on 2025-06-05 10:17_

---

_Comment by @MichaReiser on 2025-06-06 13:24_

I close this because I'm working on another version that uses two instead of just one field

---

_Closed by @MichaReiser on 2025-06-06 13:24_

---
