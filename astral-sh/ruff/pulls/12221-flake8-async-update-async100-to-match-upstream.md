```yaml
number: 12221
title: "[`flake8-async`] Update `ASYNC100` to match upstream"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: async100
created_at: 2024-07-07T06:07:00Z
updated_at: 2024-07-09T17:59:48Z
url: https://github.com/astral-sh/ruff/pull/12221
synced_at: 2026-01-10T21:47:02Z
```

# [`flake8-async`] Update `ASYNC100` to match upstream

---

_Pull request opened by @augustelalande on 2024-07-07 06:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the name of `ASYNC100` to match [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html).

Also update to the functionality to match upstream by supporting additional context managers from asyncio and anyio. Matching this [list](https://flake8-async.readthedocs.io/en/latest/glossary.html#timeout-context).

Part of #12039.

## Test Plan

Added the new context managers to the fixture.


---

_Label `rule` added by @MichaReiser on 2024-07-08 08:04_

---

_Label `preview` added by @MichaReiser on 2024-07-08 08:04_

---

_@MichaReiser approved on 2024-07-08 08:06_

Awesome, thank you!

I think we should gate this change behind preview because it changes the scope of the rule. 

---

_Comment by @augustelalande on 2024-07-08 19:18_

Not super straight forward to gate since it's just detecting a few additional context managers. I think there's precedence for not gating when just detecting more functions, e.g., #12065

---

_Comment by @MichaReiser on 2024-07-09 07:19_

The way this differs in my view from the numpy rule is that the NumPy rule was made more complete by including more of the deprecated methods. This PR introduces support for entire new async libraries, which extends the scope of the rule. @zanieb what's your take on this?

---

_Comment by @augustelalande on 2024-07-09 17:51_

Ok no problem. I did the gating.

---

_@charliermarsh approved on 2024-07-09 17:52_

Thank you!

---

_Merged by @charliermarsh on 2024-07-09 17:55_

---

_Closed by @charliermarsh on 2024-07-09 17:55_

---

_Branch deleted on 2024-07-09 17:59_

---
