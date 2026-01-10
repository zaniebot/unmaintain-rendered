```yaml
number: 12327
title: "Support `UV_PROJECT` environment to set project directory."
type: pull_request
state: merged
author: nozomirin
labels:
  - configuration
assignees: []
merged: true
base: main
head: support_UV_PROJECT_environment_variable
created_at: 2025-03-20T04:22:08Z
updated_at: 2025-03-30T19:12:03Z
url: https://github.com/astral-sh/uv/pull/12327
synced_at: 2026-01-10T11:10:39Z
```

# Support `UV_PROJECT` environment to set project directory.

---

_Pull request opened by @nozomirin on 2025-03-20 04:22_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Support the `UV_PROJECT` environment variable to set the project directory.
#11946 

## Test Plan

<!-- How was it tested? -->
`cargo nextest run` passed except the cache_prune.
```
export UV_PROJECT=/path/to/project
uv sync
```
works.


---

_Comment by @nozomirin on 2025-03-20 04:36_

I don't know which part of the code I changed that resulted in this test error.
![image](https://github.com/user-attachments/assets/00759452-18fe-4123-b846-8594bd24b41b)
I would be grateful if there is some advice.

---

_Renamed from "Support UV_PROJECT environment to set project directory." to "Support `UV_PROJECT` environment to set project directory." by @charliermarsh on 2025-03-30 19:03_

---

_@charliermarsh approved on 2025-03-30 19:04_

---

_Label `configuration` added by @charliermarsh on 2025-03-30 19:05_

---

_Merged by @charliermarsh on 2025-03-30 19:12_

---

_Closed by @charliermarsh on 2025-03-30 19:12_

---
