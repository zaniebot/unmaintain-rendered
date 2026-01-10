```yaml
number: 1563
title: "Completions should be suppressed in `for foo<CURSOR>`, `def foo(param<CURSOR>` and `def foo[T<CURSOR>]`"
type: issue
state: closed
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-11-14T19:20:27Z
updated_at: 2025-11-21T17:06:48Z
url: https://github.com/astral-sh/ty/issues/1563
synced_at: 2026-01-10T01:58:59Z
```

# Completions should be suppressed in `for foo<CURSOR>`, `def foo(param<CURSOR>` and `def foo[T<CURSOR>]`

---

_Issue opened by @BurntSushi on 2025-11-14 19:20_

### Summary

In https://github.com/astral-sh/ruff/pull/21460, we suppressed completions after an `as` statement. This resolved the main issue cited in #1287, but there are still a few cases remaining (cited in the title) where we should suppress completions.

(In general, any time we're in a context where a new name is being introduced, we should suppress completions.)

Originally cited at: https://github.com/astral-sh/ty/issues/1287#issuecomment-3527860427

### Version

_No response_

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 19:20_

---

_Label `server` added by @BurntSushi on 2025-11-14 19:20_

---

_Label `completions` added by @BurntSushi on 2025-11-14 19:20_

---

_Comment by @pyscripter on 2025-11-14 21:46_

def foo^ is already done in the previous issue.  for can be very easily added.

```rust
fn is_in_definition_place(db: &dyn Db, file: File, tokens: &[Token], typed: Option<&str>) -> bool {
    fn is_definition_token(token: &Token) -> bool {
        matches!(
            token.kind(),
            TokenKind::Def | TokenKind::Class | TokenKind::Type | TokenKind::As
        )
    }
```

---

_Comment by @BurntSushi on 2025-11-14 21:50_

This isn't for `def foo<CURSOR>`. It's for `def foo(param<CURSOR>`.

---

_Closed by @BurntSushi on 2025-11-21 17:06_

---
