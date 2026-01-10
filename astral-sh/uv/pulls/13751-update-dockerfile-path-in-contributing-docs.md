```yaml
number: 13751
title: Update dockerfile path in contributing docs
type: pull_request
state: merged
author: eegli
labels: []
assignees: []
merged: true
base: main
head: update-contributing-link
created_at: 2025-05-31T08:14:01Z
updated_at: 2025-06-02T08:16:20Z
url: https://github.com/astral-sh/uv/pull/13751
synced_at: 2026-01-10T11:10:42Z
```

# Update dockerfile path in contributing docs

---

_Pull request opened by @eegli on 2025-05-31 08:14_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Sets the correct path to the builder dockerfile in the contributing docs.


---

_Renamed from "docs: update dockerfile command in contributing docs" to "Update dockerfile path in contributing docs" by @eegli on 2025-05-31 08:14_

---

_Review requested from @konstin by @zanieb on 2025-05-31 14:56_

---

_@konstin reviewed on 2025-06-02 07:52_

---

_Review comment by @konstin on `CONTRIBUTING.md`:77 on 2025-06-02 07:52_

With buildx now being the default, we can simplify this further:

```suggestion
$ docker build -t uv-builder -f crates/uv-dev/builder.dockerfile --load .
```

---

_@konstin approved on 2025-06-02 07:52_

thanks!

---

_Merged by @konstin on 2025-06-02 08:16_

---

_Closed by @konstin on 2025-06-02 08:16_

---
