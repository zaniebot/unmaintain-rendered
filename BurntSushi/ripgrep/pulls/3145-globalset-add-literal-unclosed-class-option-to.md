```yaml
number: 3145
title: "globalset : Add literal_unclosed_class option to treat unclosed class as Literals"
type: pull_request
state: closed
author: mostafa630
labels:
  - rollup
assignees: []
base: master
head: literal_unclosed_class_opt_in_feature
created_at: 2025-09-12T20:08:58Z
updated_at: 2025-09-20T01:08:49Z
url: https://github.com/BurntSushi/ripgrep/pull/3145
synced_at: 2026-01-12T18:23:15Z
```

# globalset : Add literal_unclosed_class option to treat unclosed class as Literals

---

_@mostafa630_

## Changes
- Added `literal_unclosed_class: bool` to `GlobOptions` (defaults to `false`)
- Added `literal_unclosed_class()` method to `GlobBuilder`
- Modified `parse_class()` to save parser state and rollback on `UnclosedClass` errors when option is enabled
- Added comprehensive tests for patterns like `[abc`, `[]`, `[!]`, `[!`

## Behavior
When enabled, patterns that would throw `ErrorKind::UnclosedClass` are treated as literal characters instead:
- `[abc`  : matches literal `[abc`
- `[]`  : matches literal `[]`
- `[!]` :  matches literal `[!]`
- ` [! ` : matches literal `[!`

---

_Comment by @BurntSushi on 2025-09-19 21:01_

This needed a fair bit more work:

1. Your approach to parsing is quadratic. I fixed this by observing that if EOF is hit without finding a `]`, then there can never be another character class in the pattern. So when this happens, a toggle is set and prevents the parser from ever calling `parse_class` again.
2. This didn't wire the change up to `ignore`, which was a bit subtle. Namely, `Gitignore` now enables this by default but `Override` does not.
3. I added more tests.
4. I cleaned up the docs.
5. I renamed this to `allow_unclosed_class`.

---

_Label `rollup` added by @BurntSushi on 2025-09-19 21:01_

---

_Comment by @mostafa630 on 2025-09-19 21:53_

Thanks a lot for including this in the rollup,
and I really appreciate the detailed feedback you shared

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
