```yaml
number: 15252
title: Make unreachable a test rule for now
type: pull_request
state: merged
author: MichaReiser
labels:
  - preview
assignees: []
merged: true
base: main
head: micha/unreachable-test-rule
created_at: 2025-01-04T11:29:23Z
updated_at: 2025-01-04T11:52:19Z
url: https://github.com/astral-sh/ruff/pull/15252
synced_at: 2026-01-12T15:55:50Z
```

# Make unreachable a test rule for now

---

_@MichaReiser_

## Summary

I want to do a bugfix release after we reverted the problematic changes with UP006 (see https://github.com/astral-sh/ruff/pull/15250) 
but I'm worried that the new unreachable rule results in infinite loops as reported here https://github.com/astral-sh/ruff/issues/15248

That's why I'm excluding the unreachable rule from the bugfix release for now. We should do some more fuzzing before promoting the rule to preview.

## Test Plan

`cargo test` and `cargo test --release`

the rule can no longer be selected: 

```shell
❯ cargo run -- check --select PLW0101
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff check --select PLW0101`
error: invalid value 'PLW0101' for '--select <RULE_CODE>'

For more information, try '--help'.
```

There are no ecosystem changes because the ecosystem tests build with the test rules feature enabled.


---

_Label `preview` added by @MichaReiser on 2025-01-04 11:30_

---

_Comment by @github-actions[bot] on 2025-01-04 11:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-01-04 11:46_

CC: @dylwil3 

---

_@AlexWaygood approved on 2025-01-04 11:49_

---

_Merged by @MichaReiser on 2025-01-04 11:52_

---

_Closed by @MichaReiser on 2025-01-04 11:52_

---

_Branch deleted on 2025-01-04 11:52_

---
