```yaml
number: 182
title: "feat: Implement `InvalidPrintSyntax` (`F633`)"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/f633
created_at: 2022-09-13T23:06:10Z
updated_at: 2022-09-14T08:12:13Z
url: https://github.com/astral-sh/ruff/pull/182
synced_at: 2026-01-12T05:48:45Z
```

# feat: Implement `InvalidPrintSyntax` (`F633`)

---

_Pull request opened by @Stranger6667 on 2022-09-13 23:06_

Hi! I tried to implement F633 - looks like it works :) Let me know what you think

---

_Comment by @charliermarsh on 2022-09-14 01:10_

Looks great -- thank you!

Only things missing were: adding to `examples/generate_rules_table.rs`, running `cargo run --example generate_rules_table`, pasting the table into the README, and adding the rule to the default list in `settings.rs`.


---

_Merged by @charliermarsh on 2022-09-14 01:10_

---

_Closed by @charliermarsh on 2022-09-14 01:10_

---

_Branch deleted on 2022-09-14 08:10_

---

_Comment by @Stranger6667 on 2022-09-14 08:12_

Thanks! Though, it looks like `pyproject.toml` has some metadata from `autobot`

---
