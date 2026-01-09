---
number: 5963
title: "fix(help): Align mixed argument types"
type: pull_request
state: merged
author: BrandonXLF
labels: []
assignees: []
merged: true
base: master
head: align-mixed
created_at: 2025-03-30T07:33:19Z
updated_at: 2025-04-11T19:32:56Z
url: https://github.com/clap-rs/clap/pull/5963
synced_at: 2026-01-07T13:12:20-06:00
---

# fix(help): Align mixed argument types

---

_Pull request opened by @BrandonXLF on 2025-03-30 07:33_

Fixes the alignment of sections with mixed argument types, including those without short arguments. This is done by adding an extra 4 to the length of arguments that will show a short flag (by calling `fn short`) instead of trying to account for it elsewhere.

For example:

```
Options:
      --from <FROM_VER>  The version to upgrade from
  [TO_VER]           The version to upgrade to
```

Will now show as:

```
Options:
  --from <FROM_VER>  The version to upgrade from
  [TO_VER]           The version to upgrade to
```


Fixes #3835.

---

_Review comment by @epage on `clap_builder/src/output/help_template.rs`:485 on 2025-03-31 17:59_

Instead of comments to declare logical dependencies, should we pull this out into a variable?  Ideally, that would be in a refactor commit before this one

---

_@epage reviewed on 2025-03-31 17:59_

---

_@epage reviewed on 2025-03-31 18:00_

---

_Review comment by @epage on `clap_builder/src/output/help_template.rs`:482 on 2025-03-31 18:00_

I'd rather this get pulled out into a separate variable than to embed an `if` inside a parameter like this.

---

_@epage reviewed on 2025-03-31 18:00_

---

_Review comment by @epage on `clap_builder/src/output/help_template.rs`:485 on 2025-03-31 18:00_

(uggh, the code already was doing this)

---

_@epage reviewed on 2025-03-31 18:02_

---

_Review comment by @epage on `tests/builder/help.rs`:3972 on 2025-03-31 18:02_

Could you re-orgaize this PR where this commit as added in its own commit but changed so its passing, then this commit would change the test to pass with the new logic.  The diff between these two commits would then show how the behavior changed.

See https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#preparing-the-pr for more details

---

_@BrandonXLF reviewed on 2025-04-01 01:55_

---

_Review comment by @BrandonXLF on `clap_builder/src/output/help_template.rs`:485 on 2025-04-01 01:55_

This sounds like a good idea regardless, done.

---
