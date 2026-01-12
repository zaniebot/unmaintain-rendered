```yaml
number: 13635
title: "[red-knot] feat: implement all rich comparison operators"
type: pull_request
state: closed
author: Slyces
labels:
  - ty
assignees: []
draft: true
base: main
head: feat/rich-comparison-algorithm
created_at: 2024-10-04T19:52:31Z
updated_at: 2024-10-28T10:06:05Z
url: https://github.com/astral-sh/ruff/pull/13635
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] feat: implement all rich comparison operators

---

_@Slyces_

The algorithm for rich comparison still has some todo items

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The algorithm to perform rich comparisons on instances is non trivial. #13571 introduced a placeholder for `__lt__`.

This PR addresses some of the `// TODO` and contributes towards #13618.

## Test Plan

To be done, this is why the PR is still a draft


---

_Label `red-knot` added by @AlexWaygood on 2024-10-07 18:28_

---

_Comment by @carljm on 2024-10-17 16:51_

We may want to get this in place soon; let us know if you'd like to continue with it, or if it would be better for one of us to pick it up and get it ready to land!

---

_Comment by @Slyces on 2024-10-17 18:59_

Hi, I've been a bit busy with work recently, I don't mind if someone picks the ticket!

Do you want me to close the draft PR?

---

_Comment by @carljm on 2024-10-17 21:14_

No problem, thanks for replying! (And for all your contributions over the last few weeks.)

No need to close this PR, it may be useful reference / starting point. But (until/unless we hear otherwise) we'll assume that you aren't working on it and it's safe for one of us to pick it up from here, without duplicating work.

---

_Comment by @cake-monotone on 2024-10-21 11:22_

If it’s alright with you, would it be okay for me to continue with this work?

---

_Comment by @carljm on 2024-10-21 18:06_

> If it’s alright with you, would it be okay for me to continue with this work?

I think @Slyces already clarified above that this is open for someone else to pick up, so feel free to go ahead. And thank you!!

---

_Comment by @AlexWaygood on 2024-10-26 18:25_

Implemented by @cake-monotone in https://github.com/astral-sh/ruff/pull/13903. Thanks so much for your work here, really appreciate it!!

---

_Closed by @AlexWaygood on 2024-10-26 18:25_

---

_Branch deleted on 2024-10-28 10:06_

---
