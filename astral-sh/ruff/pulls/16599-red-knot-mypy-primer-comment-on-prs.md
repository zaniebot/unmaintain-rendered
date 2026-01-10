```yaml
number: 16599
title: "[red-knot] mypy_primer: comment on PRs"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-pr-comment
created_at: 2025-03-10T13:17:00Z
updated_at: 2025-03-10T14:26:54Z
url: https://github.com/astral-sh/ruff/pull/16599
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] mypy_primer: comment on PRs

---

_Pull request opened by @sharkdp on 2025-03-10 13:17_

## Summary

Add a new pipeline to comment on PRs if there is a mypy_primer diff result.

## Test Plan

Not yet, I'm afraid I will have to merge this first to have the pipeline available on main.

---

_Label `ci` added by @sharkdp on 2025-03-10 13:17_

---

_Label `red-knot` added by @sharkdp on 2025-03-10 13:17_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 13:40_

Could we avoid duplicating this entire workflow and instead use the existing workflow?

---

_@MichaReiser reviewed on 2025-03-10 13:41_

---

_@sharkdp reviewed on 2025-03-10 13:55_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 13:55_

What if there are ruff-ecosystem and mypy_primer ecosystem changes (e.g. for changes in the parser / AST)? Should the generic workflow be triggered twice and manage two separate comments? Or should it be be triggered once and then loop internally? Or should it be triggered once and even try to merge those changes into a single PR comment?

Even in the simplest case (triggered twice), this will require quite a bit of parametrization (from which workflow was this triggered? where do we find the artifacts? what is the artifact filename? what is the special tag to find the comment? …) and branching due to custom logic in the "Generate comment content" step.

I can attempt to do this, but I'd like to know the desired outcome first, because my GitHub actions "skills" are quite underdeveloped, so this will probably take me some time :upside_down_face:.

---

_@MichaReiser reviewed on 2025-03-10 14:13_

---

_Review comment by @MichaReiser on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 14:13_

Hmm, that's a good point. I didn't consider that we may have to run both. So maybe it's fine to keep it as is for now

---

_Marked ready for review by @sharkdp on 2025-03-10 14:17_

---

_@AlexWaygood reviewed on 2025-03-10 14:22_

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 14:22_

There are also advantages to keeping it similar to the mypy_primer workflow at typeshed/pyright/mypy, since I'm fairly familiar with the workflows in those repositories, and Shantanu is very familiar with the workflows in those repositories (and I'm sure Shantanu would be happy to help if we have issues!)

---

_@sharkdp reviewed on 2025-03-10 14:24_

---

_Review comment by @sharkdp on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 14:24_

I didn't copy over the typeshed/pyright/mypy workflow because I'm not sure that the sharding will be helpful. At least for now, the bottleneck is building Red Knot, not running it across on projects.

---

_Comment by @sharkdp on 2025-03-10 14:25_

Merging this to test it. Happy to make adjustments later.

---

_Merged by @sharkdp on 2025-03-10 14:25_

---

_Closed by @sharkdp on 2025-03-10 14:25_

---

_Branch deleted on 2025-03-10 14:25_

---

_@AlexWaygood reviewed on 2025-03-10 14:26_

---

_Review comment by @AlexWaygood on `.github/workflows/mypy_primer_comment.yaml`:1 on 2025-03-10 14:26_

Yeah, pyright initially didn't use sharding either. They also started off with a very minimal set of projects to run pyright on, but Shantanu and Eric worked together to expand it over time. After the list of projects grew large, pyright started using sharding as well.

---
