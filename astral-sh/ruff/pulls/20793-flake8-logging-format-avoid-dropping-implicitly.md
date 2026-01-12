```yaml
number: 20793
title: "[`flake8-logging-format`] Avoid dropping implicitly concatenated pieces in the `G004` fix"
type: pull_request
state: merged
author: pieterh-oai
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pieterh-oai/G004-implicit-concat
created_at: 2025-10-09T21:33:02Z
updated_at: 2025-10-28T18:49:02Z
url: https://github.com/astral-sh/ruff/pull/20793
synced_at: 2026-01-12T15:57:10Z
```

# [`flake8-logging-format`] Avoid dropping implicitly concatenated pieces in the `G004` fix

---

_@pieterh-oai_

## Summary

The original autofix for G004 was quietly dropping everything but the f-string components of any implicit concatenation sequence; this addresses that.

Side note: It looks like `f_strings` is a bit risky to use (since it implicitly skips non-f-string parts); use iter and include implicitly concatenated pieces. We should consider if it's worth having (convenience vs. bit risky). 

## Test Plan

```
cargo test -p ruff_linter
```

Backtest (run new testcases against previous implementation):
```
git checkout HEAD^ crates/ruff_linter/src/rules/flake8_logging_format/rules/logging_call.rs
cargo test -p ruff_linter

```

---

_Comment by @github-actions[bot] on 2025-10-09 21:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-10-09 22:01_

---

_Label `fixes` added by @ntBre on 2025-10-09 22:01_

---

_Label `preview` added by @ntBre on 2025-10-09 22:01_

---

_@ntBre approved on 2025-10-09 22:08_

Very nice, thank you!

Yeah, `f_strings` does seem a bit risky to use, I certainly didn't realize this when reviewing the fix initially. I quickly went through the other references, and they look correct at least.

It also seems a bit unfortunate that we join the implicitly concatenated parts, but we could follow up on that separately.

---

_Renamed from "[ruff_linter][G004 autofix] drops implicit concat pieces in the format string -> fix" to "[`flake8-logging-format`] Avoid dropping implicitly concatenated pieces in the `G004` fix" by @ntBre on 2025-10-09 22:14_

---

_Merged by @ntBre on 2025-10-09 22:14_

---

_Closed by @ntBre on 2025-10-09 22:14_

---

_Comment by @pieterh-oai on 2025-10-09 23:27_

Thanks for the quick review @ntBre !

---

_Branch deleted on 2025-10-28 18:49_

---
