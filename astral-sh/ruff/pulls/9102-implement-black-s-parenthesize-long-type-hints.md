```yaml
number: 9102
title: "Implement Black's `parenthesize-long-type-hints` preview style"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: parenthesize-long-type-hints
created_at: 2023-12-12T03:09:03Z
updated_at: 2024-01-16T10:02:50Z
url: https://github.com/astral-sh/ruff/pull/9102
synced_at: 2026-01-10T22:57:09Z
```

# Implement Black's `parenthesize-long-type-hints` preview style

---

_Pull request opened by @MichaReiser on 2023-12-12 03:09_

## Summary

This PR implements Black's `parenthesize_long_type_hints` preview style. 

The gist of the new formatting is to parenthesize and indent long-type annotations. 

Closes #8894

## Test Plan

TODO


---

_Comment by @MichaReiser on 2023-12-12 03:09_

Current dependencies on/for this PR:
* main
  * **PR #8920** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8943** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #9102** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #8941** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #8940** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment).

---

_Comment by @MichaReiser on 2023-12-21 01:44_

We're putting the implementation of `parenthesizing-long-type-hints` in function definitions on hold because we aren't convinced it is the right way to fix the underlying issue. We'll share more details soon.

---

_Closed by @MichaReiser on 2023-12-21 01:44_

---

_Branch deleted on 2024-01-16 10:02_

---
