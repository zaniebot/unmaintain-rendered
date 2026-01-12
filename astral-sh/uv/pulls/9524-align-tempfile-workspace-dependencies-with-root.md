```yaml
number: 9524
title: Align tempfile workspace dependencies with root project
type: pull_request
state: merged
author: Coruscant11
labels:
  - internal
assignees: []
merged: true
base: main
head: adjust-tempfile-dependency
created_at: 2024-11-29T15:27:15Z
updated_at: 2024-11-29T17:05:10Z
url: https://github.com/astral-sh/uv/pull/9524
synced_at: 2026-01-12T16:08:50Z
```

# Align tempfile workspace dependencies with root project

---

_@Coruscant11_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
While working on potential bug fixes with temporary files on Windows (I think I am currently ecountering the same issue as #2810)
I noticed that sub-workspaces were not all having the same `tempfile` version. And they were not relying on the cargo root project dependency. I don't know at all if it was done on purpose or not.
(I also wanted to override the root dependency with a local source but it was not possible due to sub-workspaces not relying on the same).

The root lockfile already pinned to the `3.14.0`. Some sub-workspaces were depending on the `3.12.0`, some others on the `3.14.0`. So I updated the root `Cargo.toml` to the `3.14.0`.

Feel free to decline if it was done on purpose! No worries at all :slightly_smiling_face: 

Thanks!

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
All units tests are still passing on my side. Let's see with the pull-request CI :smile: 


---

_Renamed from "Align workspace dependencies for tempfile with root" to "Align tempfile workspace dependencies with root project" by @Coruscant11 on 2024-11-29 15:31_

---

_@charliermarsh approved on 2024-11-29 17:05_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-11-29 17:05_

---

_Merged by @charliermarsh on 2024-11-29 17:05_

---

_Closed by @charliermarsh on 2024-11-29 17:05_

---
