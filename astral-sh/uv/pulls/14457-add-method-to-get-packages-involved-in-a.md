```yaml
number: 14457
title: "Add method to get packages involved in a `NoSolutionError`"
type: pull_request
state: merged
author: tdejager
labels: []
assignees: []
merged: true
base: main
head: package-names-from-error
created_at: 2025-07-04T08:47:26Z
updated_at: 2025-07-04T18:08:24Z
url: https://github.com/astral-sh/uv/pull/14457
synced_at: 2026-01-12T16:11:13Z
```

# Add method to get packages involved in a `NoSolutionError`

---

_@tdejager_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

In pixi we overlay the PyPI packages over the conda packages and we sometimes need to figure out what PyPI packages are involved in the no-solution error. We could parse the error message, but this is pretty error-prone, so it would be good to get access to more information. A lot of information in this module is private and should probably stay this way, but package names are easy enough to expose. This would help us a lot!

I collect into a HashSet to remove duplication, and did not want to expose a rustc_hash datastructure directly, thats's why I've chosen to expose as an iterator :)

Let me know if any changes need to be done, and thanks!




---

_@zanieb approved on 2025-07-04 17:56_

---

_Merged by @zanieb on 2025-07-04 18:08_

---

_Closed by @zanieb on 2025-07-04 18:08_

---
