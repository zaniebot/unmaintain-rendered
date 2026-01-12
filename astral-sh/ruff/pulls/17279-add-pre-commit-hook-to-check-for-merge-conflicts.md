```yaml
number: 17279
title: Add pre-commit hook to check for merge conflicts
type: pull_request
state: merged
author: Skylion007
labels:
  - ci
assignees: []
merged: true
base: main
head: skylion007/add-merge-conflict-check-2025-04-07
created_at: 2025-04-07T15:12:40Z
updated_at: 2025-04-10T10:47:16Z
url: https://github.com/astral-sh/ruff/pull/17279
synced_at: 2026-01-12T15:56:01Z
```

# Add pre-commit hook to check for merge conflicts

---

_@Skylion007_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.) Yes
- Does this pull request include a descriptive title? Yes
- Does this pull request include references to any relevant issues? Yes
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
* Adds a pre-commit check for merge conflict inspired by reviewing #17278. This will flag any commits that contain merge-conflict strings before they are pushed to branches.

## Test Plan
* Checks that it flags merge conflict like in #17278 . This is one of the builtin standard hooks and I have used it in many repos.
<!-- How was it tested? -->


---

_Comment by @MichaReiser on 2025-04-07 16:08_

This is interesting. How fast is this pre-commit step? I'm a bit annoyed by how slow our pre-commit is if I use `--all-files` (I have to use `--all-files` because I never run pre-commit on individual commits, we aren't friends ;)) 

I guess my concern is, I don't want to make my workflow significantly slower and this is already something that otherwise gets catched in CI

---

_Comment by @AlexWaygood on 2025-04-07 16:27_

I'm also a bit sceptical this will be that useful -- it's a pretty fast hook IIRC, but I'm not convinced it'll be that useful given that merge conflicts are pretty easy to catch in CI through our existing checks. And I agree that our pre-commit is already somewhat slow locally.

---

_Comment by @Skylion007 on 2025-04-07 17:58_

In my experience, it's a super fast lightweight hook; and one of the standard pre-commit hooks. The builtin hooks are pretty optimized.

This also catches merge conflicts in code areas that are not run: like say documentation, text files or other file types.

---

_Comment by @AlexWaygood on 2025-04-10 10:22_

Okay, I checked out this PR locally and the new hook is basically instantaneous on this repo. So if this will help some of our contributors, let's go for it.

Thanks!

---

_Merged by @AlexWaygood on 2025-04-10 10:22_

---

_Closed by @AlexWaygood on 2025-04-10 10:22_

---

_Label `ci` added by @AlexWaygood on 2025-04-10 10:47_

---
