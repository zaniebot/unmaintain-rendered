---
number: 17040
title: Update UV_VERSION in docs for GitLab CI/CD
type: pull_request
state: closed
author: npikall
labels:
  - documentation
assignees: []
base: main
head: dev
created_at: 2025-12-08T23:34:41Z
updated_at: 2025-12-09T20:56:33Z
url: https://github.com/astral-sh/uv/pull/17040
synced_at: 2026-01-07T13:12:19-06:00
---

# Update UV_VERSION in docs for GitLab CI/CD

---

_Pull request opened by @npikall on 2025-12-08 23:34_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
## Summary
Update the `UV_VERSION`, such that a `copy-to-clipboard` action and pasting into a `.gitlab-ci.yml` is not 4 minor versions behind, as it happened to me a couple of times.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
I ran `mkdocs serve` and it worked (I literally only changed one character)
<!-- How was it tested? -->


---

_@samypr100 reviewed on 2025-12-09 03:22_

---

_Review comment by @samypr100 on `docs/guides/integration/gitlab.md`:16 on 2025-12-09 03:22_

```suggestion
  UV_VERSION: "0.9.16"
```

We shouldalso add it in https://github.com/astral-sh/uv/blob/a63e5b62e39c7f581831daaaa2df4a21ae27a017/pyproject.toml#L74 so it auto-increments on release.

---

_Review comment by @npikall on `docs/guides/integration/gitlab.md`:16 on 2025-12-09 08:20_

@samypr100 Great, I hope this works like this (I am not quite familiar with `rooster`)

---

_@npikall reviewed on 2025-12-09 08:20_

---

_@zanieb approved on 2025-12-09 20:56_

---

_Merged by @zanieb on 2025-12-09 20:56_

---

_Closed by @zanieb on 2025-12-09 20:56_

---

_Label `documentation` added by @zanieb on 2025-12-09 20:56_

---

_Referenced in [Homebrew/homebrew-core#257965](../../Homebrew/homebrew-core/pulls/257965.md) on 2025-12-09 23:22_

---
