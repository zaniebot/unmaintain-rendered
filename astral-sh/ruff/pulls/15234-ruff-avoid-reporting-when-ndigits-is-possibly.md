```yaml
number: 15234
title: "[`ruff`] Avoid reporting when `ndigits` is possibly negative (`RUF057`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: RUF057
created_at: 2025-01-03T05:06:54Z
updated_at: 2025-01-03T22:35:24Z
url: https://github.com/astral-sh/ruff/pull/15234
synced_at: 2026-01-12T15:55:50Z
```

# [`ruff`] Avoid reporting when `ndigits` is possibly negative (`RUF057`)

---

_@InSyncWithFoo_

## Summary

Resolves #15221.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:118 on 2025-01-03 05:09_

What do you think about `LiteralPositiveInt` instead to avoid the double negative?

---

_@dhruvmanila reviewed on 2025-01-03 05:09_

---

_Renamed from "[`ruff`] Avoid fixing when `ndigits` is possibly negative (`RUF057`)" to "[`ruff`] Avoid reporting when `ndigits` is possibly negative (`RUF057`)" by @InSyncWithFoo on 2025-01-03 05:09_

---

_Label `bug` added by @dhruvmanila on 2025-01-03 05:12_

---

_Label `preview` added by @dhruvmanila on 2025-01-03 05:12_

---

_@InSyncWithFoo reviewed on 2025-01-03 05:15_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:118 on 2025-01-03 05:15_

I don't think there's any double negative here. Besides, 0 is not positive:

```pycon
>>> round(42, 0)
42
```

---

_Comment by @github-actions[bot] on 2025-01-03 05:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-03 07:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:118 on 2025-01-03 07:44_

I'd go with. It simplifies the matches where the negative flag isn't relevant and it avoids the `NotNegative` (which feels like a double negative)

```suggestion
    LiteralInt { negative: bool }
```

---

_@MichaReiser reviewed on 2025-01-03 07:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:119 on 2025-01-03 07:46_

I'd prefer keeping `Int` here to keep it aligned with `RoundedValue` and the current name is also clearer because does the new name mean *it's not a Literal but an int* or *everything else other than an integer literal*. I think it's the former, but the name doesn't make it clear. 

---

_Comment by @MichaReiser on 2025-01-03 09:13_

Thanks for fixing this so quickly!

---

_@MichaReiser approved on 2025-01-03 18:47_

---

_Merged by @MichaReiser on 2025-01-03 18:48_

---

_Closed by @MichaReiser on 2025-01-03 18:48_

---

_Branch deleted on 2025-01-03 20:52_

---

_Comment by @UnknownPlatypus on 2025-01-03 22:35_

Lovely, thank you @InSyncWithFoo 

---
