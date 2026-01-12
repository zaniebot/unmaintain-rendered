```yaml
number: 3154
title: "printer: hack in a fix for `-l/--files-with-matches` when using `--pcre2 --multiline` with look-around"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-i3139
created_at: 2025-09-21T18:01:56Z
updated_at: 2025-09-22T13:12:18Z
url: https://github.com/BurntSushi/ripgrep/pull/3154
synced_at: 2026-01-12T18:23:15Z
```

# printer: hack in a fix for `-l/--files-with-matches` when using `--pcre2 --multiline` with look-around

---

_@BurntSushi_

The underlying issue here is #2528, which was introduced by commit
efd9cfb2fc1f0233de9eda4c03416d32ef2c3ce8 which fixed another bug.

For the specific case of "did a file match," we can always assume the
match count is at least 1 here. But this doesn't fix the underlying
problem.

Fixes #3139


---

_Merged by @BurntSushi on 2025-09-22 13:12_

---

_Closed by @BurntSushi on 2025-09-22 13:12_

---

_Branch deleted on 2025-09-22 13:12_

---
