```yaml
number: 2792
title: Split commands.rs into separate files
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/commands
created_at: 2023-02-12T02:55:38Z
updated_at: 2023-02-12T06:24:58Z
url: https://github.com/astral-sh/ruff/pull/2792
synced_at: 2026-01-12T04:52:01Z
```

# Split commands.rs into separate files

---

_Pull request opened by @charliermarsh on 2023-02-12 02:55_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-02-12 02:55_

---

_Merged by @charliermarsh on 2023-02-12 02:58_

---

_Closed by @charliermarsh on 2023-02-12 02:58_

---

_Branch deleted on 2023-02-12 02:58_

---

_@not-my-profile reviewed on 2023-02-12 03:53_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/commands/mod.rs`:17 on 2023-02-12 03:53_

Just a note that in general I prefer to make the modules public and use e.g. `commands::rule::rule` in `main.rs`. (That same preference of mine also applies to rule modules.) I think re-exporting can make sense when it's a public API but this isn't one and neither are our rule modules.

---

_@charliermarsh reviewed on 2023-02-12 04:22_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/mod.rs`:17 on 2023-02-12 04:22_

I'm not disagreeing, but why do you prefer that style? (Just trying to learn. I wasn't really sure myself what was preferable.)

(The one thing I _dislike_ is when there are multiple ways to import the same member due to a re-export, e.g., you can import `RuleSelector` via `use crate::rule_selector::RuleSelector;` or `use crate::RuleSelector;` in `ruff`, but I think that may be hard to avoid in that case.)


---

_@not-my-profile reviewed on 2023-02-12 06:24_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/commands/mod.rs`:17 on 2023-02-12 06:24_

I think just because there's no benefit to it in private APIs and one line is better than two lines.^^

I think it does improve code readability a bit because now you can just see in `main.rs` where exactly these methods are defined.

(btw thanks for changing this in #2801)

---
