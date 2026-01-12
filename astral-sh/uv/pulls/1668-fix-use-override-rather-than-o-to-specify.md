```yaml
number: 1668
title: "fix: use --override rather than -o to specify overrides in README.md"
type: pull_request
state: merged
author: kopp
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-02-18T22:28:23Z
updated_at: 2024-02-18T23:55:28Z
url: https://github.com/astral-sh/uv/pull/1668
synced_at: 2026-01-12T16:04:41Z
```

# fix: use --override rather than -o to specify overrides in README.md

---

_@kopp_


## Summary

Fix documentation (Readme.md):
`-o` is the short for `--output-file`, so using `-o overrides.txt` would store the output in `overrides.txt` rather than using that as overrides.

## Test Plan

no tests for docs (yet?)


---

_Comment by @kopp on 2024-02-18 22:32_

@charliermarsh : can you take a brief look at this, please?

---

_@zanieb approved on 2024-02-18 23:55_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-02-18 23:55_

---

_Merged by @zanieb on 2024-02-18 23:55_

---

_Closed by @zanieb on 2024-02-18 23:55_

---
