```yaml
number: 17653
title: "[`pydocstyle`] Add fix safety section (`D200`) "
type: pull_request
state: closed
author: Kalmaegi
labels:
  - documentation
assignees: []
base: main
head: doc_fix_safety_for_one_line
created_at: 2025-04-27T09:46:22Z
updated_at: 2025-04-28T04:34:54Z
url: https://github.com/astral-sh/ruff/pull/17653
synced_at: 2026-01-12T15:56:03Z
```

# [`pydocstyle`] Add fix safety section (`D200`) 

---

_@Kalmaegi_


## Summary

This PR add the `fix safety` section for rule `D200` in `one_liner.rs` for #15584 

I tested several cases, and the rule is generally safe, but there's a possibility that after merging, the length might exceed the project's current single-line length limit.


---

_Label `documentation` added by @MichaReiser on 2025-04-27 09:52_

---

_Comment by @VascoSch92 on 2025-04-27 20:49_

I think the section was already added in #17502

---

_Closed by @Kalmaegi on 2025-04-28 04:34_

---
