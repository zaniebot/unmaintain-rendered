```yaml
number: 682
title: add fixes for __future__ import removal
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: fixes-future-imports
created_at: 2022-11-11T12:53:54Z
updated_at: 2022-11-12T16:28:05Z
url: https://github.com/astral-sh/ruff/pull/682
synced_at: 2026-01-12T05:48:45Z
```

# add fixes for __future__ import removal

---

_Pull request opened by @chammika-become on 2022-11-11 12:53_

This PR will close #312

- Added Fixes to remove detected imports
- Added more test fixtures
- Added `UnnecessaryFutureImports` to `fixable`
- Updated README

Detection/fix code is elevated to the `Stmt` level since some imports need to be fully removed. Earlier detection at `alias` level doesn't seem right for that. As suggested, fix code is heavily borrowed from `pyflakes::fixes::remove_unused_import_froms`, perhaps need to refactor in future to share code. 
One main thing to object to, is that fix code pretty much do the detection all over again.
@charliermarsh is there better way to share detection results for fix purposes other than to display messages ?


---

_Review comment by @charliermarsh on `src/checks.rs`:1469 on 2022-11-11 16:21_

Nit: can we make this `names` and take a vector, rather than a pre-formatted string? Then we can split the check to have proper pluralization, like:

```rust
CheckKind::DuplicateHandlerException(names) => {
    if names.len() == 1 {
        let name = &names[0];
        format!("Exception handler with duplicate exception: `{name}`")
    } else {
        let names = names.iter().map(|name| format!("`{name}`")).join(", ");
        format!("Exception handler with duplicate exceptions: {names}")
    }
}
```

(Using a vector as the argument, rather than a pre-formatted string, will also yield a nicer fixture file, since the list of names will be structured data rather than a comma-joined string.)

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:187 on 2022-11-11 16:23_

I know you mentioned this in the PR summary, but we should try to avoid having this logic in two places (right now it's here, but also in `pyupgrade/checks.rs`).

How bout we remove `pyupgrade/checks.rs`, move the detection logic into `pyupgrade/plugins/unnecessary_future_import.rs` directly, and then pass in the list of names to remove as an argument to this function directly?

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:199 on 2022-11-11 16:25_

We do need to pass in `parent` and `deleted` here, as in `remove_unused_import_froms`.

---

_@charliermarsh reviewed on 2022-11-11 16:26_

Great! Some feedback below.

---

_Review comment by @chammika-become on `src/pyupgrade/fixes.rs`:187 on 2022-11-12 15:04_

Make sense. Removed from `checks` and moved to the `plugins`

---

_@chammika-become reviewed on 2022-11-12 15:04_

---

_@chammika-become reviewed on 2022-11-12 15:07_

---

_Review comment by @chammika-become on `src/pyupgrade/fixes.rs`:199 on 2022-11-12 15:07_

Got the `deleted`, I am at lost about how to get the `parent`

---

_@charliermarsh reviewed on 2022-11-12 15:25_

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:199 on 2022-11-12 15:25_

All good, I’ll take a look and fix before merging :)

---

_Marked ready for review by @charliermarsh on 2022-11-12 15:25_

---

_Merged by @charliermarsh on 2022-11-12 16:28_

---

_Closed by @charliermarsh on 2022-11-12 16:28_

---
