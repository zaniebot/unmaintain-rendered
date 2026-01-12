```yaml
number: 2756
title: "chore(git): ignore Intellij settings"
type: pull_request
state: closed
author: stegayet
labels: []
assignees: []
base: main
head: chore/ignore-idea-folder
created_at: 2024-04-01T13:43:45Z
updated_at: 2024-04-01T16:21:34Z
url: https://github.com/astral-sh/uv/pull/2756
synced_at: 2026-01-12T16:05:12Z
```

# chore(git): ignore Intellij settings

---

_@stegayet_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds `.idea/` to `.gitignore`.

## Test Plan

None.


---

_Marked ready for review by @stegayet on 2024-04-01 13:48_

---

_Comment by @zanieb on 2024-04-01 14:38_

Thanks for contributing!

I would highly recommend putting these in a [user-level gitignore](https://stackoverflow.com/questions/5724455/can-i-make-a-user-specific-gitignore-file) instead.

I prefer not to include things unrelated to the repository itself in its gitignore.

---

_Closed by @stegayet on 2024-04-01 16:21_

---

_Branch deleted on 2024-04-01 16:21_

---
