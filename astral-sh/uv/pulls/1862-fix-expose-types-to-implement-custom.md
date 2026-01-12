```yaml
number: 1862
title: "fix: expose types to implement custom `ResolverProvider`"
type: pull_request
state: merged
author: baszalmstra
labels:
  - rustlib
assignees: []
merged: true
base: main
head: fix/expose_more_types
created_at: 2024-02-22T12:47:21Z
updated_at: 2024-02-22T14:59:03Z
url: https://github.com/astral-sh/uv/pull/1862
synced_at: 2026-01-12T16:04:45Z
```

# fix: expose types to implement custom `ResolverProvider`

---

_@baszalmstra_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

To integrate `uv` into `pixi` I need to specify a custom `ResolverProvider` to be able to specify that some packages are already installed by conda and should not be touched. However, some of the types required to implement your own `ResolverProvider` were not accessible through the public API. This PR basically adds them.

## Test Plan

I didnt add an explicit test for this.


---

_@konstin approved on 2024-02-22 12:53_

---

_@zanieb approved on 2024-02-22 14:57_

---

_Label `rustlib` added by @zanieb on 2024-02-22 14:59_

---

_Merged by @zanieb on 2024-02-22 14:59_

---

_Closed by @zanieb on 2024-02-22 14:59_

---
