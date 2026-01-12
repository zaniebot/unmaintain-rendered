```yaml
number: 19209
title: "refactor(ruff_workspace): remove macOS deprecation warning "
type: pull_request
state: closed
author: CodeMan62
labels: []
assignees: []
draft: true
base: main
head: codeman/19145
created_at: 2025-07-08T15:22:04Z
updated_at: 2025-07-08T15:34:22Z
url: https://github.com/astral-sh/ruff/pull/19209
synced_at: 2026-01-12T15:56:34Z
```

# refactor(ruff_workspace): remove macOS deprecation warning 

---

_@CodeMan62_

## Summary
In https://github.com/astral-sh/ruff/pull/11115 we moved from defaulting to $HOME/Library/Application Support to $XDG_HOME on macOS and added a deprecation warning if we find files in the old location. So this PR removes the warning 
closes #19145 
## Test Plan
Ran `cargo test` 



---

_Review requested from @AlexWaygood by @CodeMan62 on 2025-07-08 15:22_

---

_Converted to draft by @CodeMan62 on 2025-07-08 15:22_

---

_Comment by @CodeMan62 on 2025-07-08 15:23_

Sorry something went wrong here creating a new PR  please check #19210 

---

_Closed by @CodeMan62 on 2025-07-08 15:23_

---

_Branch deleted on 2025-07-08 15:24_

---
