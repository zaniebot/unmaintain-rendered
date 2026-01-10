```yaml
number: 15321
title: Update build-backend.md
type: pull_request
state: open
author: RafalSkolasinski
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-08-15T22:59:32Z
updated_at: 2025-08-17T21:27:37Z
url: https://github.com/astral-sh/uv/pull/15321
synced_at: 2026-01-10T06:44:33Z
```

# Update build-backend.md

---

_Pull request opened by @RafalSkolasinski on 2025-08-15 22:59_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `--lib` flag allows to create initial project structure with `[build-system]` section and `src` folder.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2025-08-15 23:03_

---

_Review comment by @zanieb on `docs/concepts/build-backend.md`:47 on 2025-08-15 23:03_

Thank you!

I think we probably want `--package` here. Though the case could be made for either?

---

_@RafalSkolasinski reviewed on 2025-08-16 16:51_

---

_Review comment by @RafalSkolasinski on `docs/concepts/build-backend.md`:47 on 2025-08-16 16:51_

Didn't notice there is also `--package`. A bit nuanced difference between package or library in my opinion. I see one comes out with `project.scripts` section. Fact that I can combine `--lib` with `--package` makes it a bit more confusing even...  

I'll leave it up to you. Definitely bare `init` was not enough but which one you'd like to go with I don't know.

---

_@RafalSkolasinski reviewed on 2025-08-17 21:27_

---

_Review comment by @RafalSkolasinski on `docs/concepts/build-backend.md`:47 on 2025-08-17 21:27_

I applied the change

---
