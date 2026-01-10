```yaml
number: 11794
title: "gitignore `.DS_STORE` files"
type: pull_request
state: closed
author: AlexWaygood
labels: []
assignees: []
base: main
head: go-away-dsstore
created_at: 2024-06-07T09:53:31Z
updated_at: 2024-06-07T10:17:21Z
url: https://github.com/astral-sh/ruff/pull/11794
synced_at: 2026-01-10T21:56:00Z
```

# gitignore `.DS_STORE` files

---

_Pull request opened by @AlexWaygood on 2024-06-07 09:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

It's really annoying to have a dirty working tree just because I opened a Ruff subdirectory in Finder

## Test Plan

I opened some subdirectories of Ruff with Finder and the `.DS_STORE` files that were added by Finder were no longer tracked by git.


---

_Comment by @AlexWaygood on 2024-06-07 10:17_

Better to just `.gitignore` project-specific files in our project's `.gitignore`; this is what global `.gitignore` files are for

---

_Closed by @AlexWaygood on 2024-06-07 10:17_

---

_Branch deleted on 2024-06-07 10:17_

---
