```yaml
number: 6050
title: "fix: Require validation not bypassed by mutually exclusive groups"
type: pull_request
state: open
author: 12yanogden
labels: []
assignees: []
draft: true
base: master
head: issue-4707
created_at: 2025-06-27T12:54:13Z
updated_at: 2025-07-03T16:34:31Z
url: https://github.com/clap-rs/clap/pull/6050
synced_at: 2026-01-12T16:14:17Z
```

# fix: Require validation not bypassed by mutually exclusive groups

---

_@12yanogden_

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->

## Problem

[Fixes #4707](https://github.com/clap-rs/clap/issues/4707)

Previously, arguments required via `requires` could be bypassed when they conflicted with other present arguments in mutually exclusive groups.

### Example Issue:
```rust
#[derive(Parser)]
#[clap(group = ArgGroup::new("command").multiple(false))]
struct Args {
    #[clap(long, group = "command")]
    read: bool,
    
    #[clap(long, group = "command")]  
    write: bool,
    
    #[clap(long, requires = "read")]
    show_hex: bool,
}


---

_@epage reviewed on 2025-07-03 16:04_

---

_Review comment by @epage on `clap_builder/src/parser/validator.rs`:354 on 2025-07-03 16:04_

While this might fix the immediate problem, I'm worried that this is too brittle and we can change things in other code and not realize this needs to be update as well.

---

_@epage reviewed on 2025-07-03 16:07_

---

_Review comment by @epage on `tests/builder/issue_4707.rs`:1 on 2025-07-03 16:07_

Note that #4520 is the core issue and it hasn't been accepted yet, requiring further analysis for correctness and whether this would constitute a breaking change.

For future contributions, please keep in mind
- We prefer to resolve issues before moving on to PRs
- Commits should be atomic, including tests passing
- We do ask for tests to be added in commits before the behavior changes but that is to show the existing behavior
- derive tests should go in the derive testsuite, not the builder one
- tests should be grouped by and named for their functionality, not issue numbers

See also https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md

---

_@12yanogden reviewed on 2025-07-03 16:26_

---

_Review comment by @12yanogden on `tests/builder/issue_4707.rs`:1 on 2025-07-03 16:26_

So wait, is 4707 not a workable issue? I have to wait until 4520 is resolved?

---

_@epage reviewed on 2025-07-03 16:34_

---

_Review comment by @epage on `tests/builder/issue_4707.rs`:1 on 2025-07-03 16:34_

They are the same concept (how should `requires` interact with conflicts) and that question needs to be addressed before either moves forward.  Rather than split *that* conversation between two related issues, I recommend we centralize it on #4520.  That doesn't mean that #4520 needs to be implemented first.  In fact, its likely that a fix for one will fix the other or be just a one or two line change.

---
