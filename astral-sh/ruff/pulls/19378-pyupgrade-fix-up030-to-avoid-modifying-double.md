```yaml
number: 19378
title: "[`pyupgrade`] Fix `UP030` to avoid modifying double curly braces in format strings"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19348
created_at: 2025-07-15T21:29:45Z
updated_at: 2025-07-29T19:31:11Z
url: https://github.com/astral-sh/ruff/pull/19378
synced_at: 2026-01-12T15:56:38Z
```

# [`pyupgrade`] Fix `UP030` to avoid modifying double curly braces in format strings

---

_@danparizher_

## Summary

Fixes #19348

---

_Comment by @github-actions[bot] on 2025-07-15 21:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @MichaReiser on 2025-07-18 13:41_

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:08_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/format_literals.rs`:136 on 2025-07-28 18:26_

At the risk of going too wild with regular expressions, I think we can just handle this directly in `FORMAT_SPECIFIER`:

```rust
static FORMAT_SPECIFIER: LazyLock<Regex> = LazyLock::new(|| {
    Regex::new(
        r"(?x)
            (?P<prefix>
                ^|[^{]|(?:\{{2})+  # preceded by nothing, a non-brace, or an even number of braces
            )
            \{                     # opening curly brace
                (?P<int>\d+)       # followed by any integer
                (?P<fmt>.*?)       # followed by any text
            }                      # followed by a closing brace
        ",
    )
    .unwrap()
});
```

While I was here, I went ahead and set verbose mode on the regex (initial `(?x)`) and moved the comments on `FORMAT_SPECIFIER` into the regex itself.

With this change, we also have to pass along the `$prefix` in the calls below. For example:

```rust
                        string.value = arena.alloc(
                            FORMAT_SPECIFIER
                                .replace_all(string.value, "$prefix{$fmt}")
                                .to_string(),
                        );
```

I almost neglected the "even number of braces" check, but cases like this are valid:

```py
"{{{0}}}".format(123)
```

I think this is verging on needing us to parse the whole format string manually, but if we can't come up with any other edge cases, I think the regex is fine. I'd recommend adding this as a fixable test case too.

---

_@ntBre reviewed on 2025-07-28 18:26_

Thanks! I think we can just extend the current regex and also handle an additional edge case at the same time.

---

_Renamed from "[`pyupgrade`] Fix UP030 to avoid modifying double curly braces in format strings" to "[`pyupgrade`] Fix `UP030` to avoid modifying double curly braces in format strings" by @ntBre on 2025-07-29 18:28_

---

_Label `bug` added by @ntBre on 2025-07-29 18:28_

---

_Merged by @ntBre on 2025-07-29 18:35_

---

_Closed by @ntBre on 2025-07-29 18:35_

---

_Branch deleted on 2025-07-29 19:31_

---
