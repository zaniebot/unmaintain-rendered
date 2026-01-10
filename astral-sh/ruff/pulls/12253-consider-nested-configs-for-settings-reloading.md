```yaml
number: 12253
title: Consider nested configs for settings reloading
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/reload-hierarchical-config
created_at: 2024-07-09T07:12:46Z
updated_at: 2024-07-12T05:09:13Z
url: https://github.com/astral-sh/ruff/pull/12253
synced_at: 2026-01-10T21:47:02Z
```

# Consider nested configs for settings reloading

---

_Pull request opened by @dhruvmanila on 2024-07-09 07:12_

## Summary

This PR fixes a bug in the settings reloading logic to consider nested configuration in a workspace.

fixes: #11766

## Test Plan

https://github.com/astral-sh/ruff/assets/67177269/69704b7b-44b9-4cc7-b5a7-376bf87c6ef4



---

_Label `bug` added by @dhruvmanila on 2024-07-09 07:31_

---

_Label `server` added by @dhruvmanila on 2024-07-09 07:32_

---

_Marked ready for review by @dhruvmanila on 2024-07-09 07:32_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-09 07:34_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:317 on 2024-07-09 07:36_

There are two main changes:
1. We loop over every workspace folder. The `range_mut` is incorrect here because the "enclosing_folder" would be within the workspace.
2. The `starts_with` call is incorrect. We should be checking if the folder is part of the workspace or not and not the other way around which will always be incorrect for nested folders.

---

_@dhruvmanila reviewed on 2024-07-09 07:36_

---

_Comment by @github-actions[bot] on 2024-07-09 07:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-07-09 07:52_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:317 on 2024-07-09 07:52_

I wonder if this is correct though: Let's say we have workspace `a` and `b` and we add a configuration to `b/foo` so that `root` = `a` and `enclosing_folder=b/foo`. We would immediately hit the `break` here. 

I think we want `self.settings.range(..enclosing_folder).rev()` and we take the paths for as long as they start with `enclosing_folder`. 

---

_@dhruvmanila reviewed on 2024-07-09 08:07_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:317 on 2024-07-09 08:07_

Oh, that's correct. Thanks. Let me update it.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:318 on 2024-07-09 13:25_

Is `break` still incorrect here? I get the impression that we could use `break` here but not entirely sure

---

_@MichaReiser approved on 2024-07-09 13:25_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:317 on 2024-07-10 05:20_

This is actually not correct either because the `root` folder can never start with an `enclosing_folder`. This is the main reason I switched the `starts_with` call in (2). Let me think about this a bit more considering your example as well.

---

_@dhruvmanila reviewed on 2024-07-10 05:20_

---

_@MichaReiser approved on 2024-07-10 12:13_

To me it's still unclear if `break` would be sufficient instead of `continue` but I'm not a 100% sure.

---

_Comment by @dhruvmanila on 2024-07-12 04:55_

> To me it's still unclear if `break` would be sufficient instead of `continue` but I'm not a 100% sure.

I thought about this more and I think `break` should be sufficient. The only place where `continue` would matter is if there are at least three workspaces with the following characteristics:
1. Should be refreshed
2. Should be `continue`
3. Config file under this workspace has been changed
The output of `range` operation should yield `[3, 2, 1]`. I can't come up with a workspace layout where this matches. The usage of `BtreeMap` avoids this kinds of cases where `continue` would be required.

---

_Merged by @dhruvmanila on 2024-07-12 05:00_

---

_Closed by @dhruvmanila on 2024-07-12 05:00_

---

_Branch deleted on 2024-07-12 05:00_

---
