```yaml
number: 1868
title: Split up the table corresponding to the pylint rules
type: pull_request
state: merged
author: tmke8
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-01-14T08:31:03Z
updated_at: 2023-01-14T16:04:49Z
url: https://github.com/astral-sh/ruff/pull/1868
synced_at: 2026-01-12T15:55:07Z
```

# Split up the table corresponding to the pylint rules

---

_@tmke8_

This makes it easier to see which rules you're enabling when selecting one of the pylint codes (like `PLC`). This also makes it clearer what those abbreviations stand for. When I first saw the pylint section, I was very confused by that, so other might be as well.

See it rendered here: https://github.com/thomkeh/ruff/blob/patch-1/README.md#pylint-plc-ple-plr-plw

---

_Comment by @not-my-profile on 2023-01-14 08:44_

The tables in `README.md` are automatically generated from the code by [ruff_dev/src/generate_rules_table.rs](https://github.com/charliermarsh/ruff/blob/main/ruff_dev/src/generate_rules_table.rs) ... so we cannot manually edit these tables because such changes would be overridden by `cargo +nightly dev generate-all`.

---

_Comment by @tmke8 on 2023-01-14 08:45_

Oh, I see. I'll try to figure out whether I can change the generating code.

---

_Converted to draft by @tmke8 on 2023-01-14 08:46_

---

_Marked ready for review by @tmke8 on 2023-01-14 12:03_

---

_Comment by @tmke8 on 2023-01-14 12:03_

Done.

---

_Comment by @charliermarsh on 2023-01-14 13:05_

Nice, this is great.

---

_Merged by @charliermarsh on 2023-01-14 13:07_

---

_Closed by @charliermarsh on 2023-01-14 13:07_

---

_Branch deleted on 2023-01-14 16:04_

---
