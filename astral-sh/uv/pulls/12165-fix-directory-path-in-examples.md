```yaml
number: 12165
title: "Fix `--directory` path in examples"
type: pull_request
state: merged
author: jatinderjit
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-03-14T08:39:45Z
updated_at: 2025-03-18T19:50:07Z
url: https://github.com/astral-sh/uv/pull/12165
synced_at: 2026-01-10T11:10:39Z
```

# Fix `--directory` path in examples

---

_Pull request opened by @jatinderjit on 2025-03-14 08:39_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The examples assume that the packages are in the project root directory.
However, they are nested inside `src`, and the commands in the examples do not work as intended.

I could not find any related issues.

## Test Plan

<!-- How was it tested? -->

I tested it by executing the commands on my terminal - Linux and Windows (PowerShell).


---

_Comment by @charliermarsh on 2025-03-14 14:05_

Hmm, I think this is correct as-is. I think the examples are intended to be run from the parent directory, the one that contains `example-pkg`.

---

_@charliermarsh reviewed on 2025-03-14 14:06_

---

_Review comment by @charliermarsh on `docs/concepts/projects/init.md`:143 on 2025-03-14 14:06_

@zanieb -- Should we remove `--directory` entirely, and just mention that these should be run from the assumed location?

---

_Comment by @jatinderjit on 2025-03-14 14:29_

> I think the examples are intended to be run from the parent directory

Aah! My bad. I `cd`-ed into the directories before executing the commands.
Executing these from the parent directory worked. Sorry for this issue.

---

_@zanieb reviewed on 2025-03-17 20:35_

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:143 on 2025-03-17 20:35_

We could, or just use `cd`

---

_@zanieb reviewed on 2025-03-17 20:36_

---

_Review comment by @zanieb on `docs/concepts/projects/init.md`:143 on 2025-03-17 20:36_

I wanted the examples to "work as written"

---

_Review requested from @zanieb by @charliermarsh on 2025-03-18 01:15_

---

_@charliermarsh reviewed on 2025-03-18 01:15_

---

_Review comment by @charliermarsh on `docs/concepts/projects/init.md`:93 on 2025-03-18 01:15_

I think this is wrong? I changed it.

---

_Merged by @charliermarsh on 2025-03-18 19:50_

---

_Closed by @charliermarsh on 2025-03-18 19:50_

---

_Label `documentation` added by @charliermarsh on 2025-03-18 19:50_

---
