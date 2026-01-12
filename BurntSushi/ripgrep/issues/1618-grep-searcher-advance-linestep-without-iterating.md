```yaml
number: 1618
title: "grep-searcher: Advance LineStep without iterating"
type: issue
state: open
author: XAMPPRocky
labels:
  - enhancement
assignees: []
created_at: 2020-06-13T18:48:12Z
updated_at: 2020-06-15T11:34:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1618
synced_at: 2026-01-12T16:13:23Z
```

# grep-searcher: Advance LineStep without iterating

---

_@XAMPPRocky_

I've been using `grep_searcher::LineStep` for a new feature in tokei that requires the level of control it provides and it has been great to use so far. The one minor nit I've come across is that is no way to advance `LineStep` without `next`, when there are times where I want to be able to skip to a certain part of the file. This is currently possible by creating a new `LineStep` that starts at that position, but it would be nice if I didn't need to re-assign just to update `LineStep`'s position.

### Example

```rust
let mut stepper = LineStep::new(b'\n', 0, lines.len());

while let Some((start, end)) = stepper.next(lines) {
    if let Some(position) = /* some parse function */ {
        // Current
        stepper = LineStep::new(b'\n', position, lines.len());
        // Proposed
        stepper.advance_to(position);
    }
}
```

---

_Comment by @BurntSushi on 2020-06-15 11:34_

This seems fine to me. A PR is welcome. The docs for `advance_to` probably need a bit of detail. e.g., If the position you advance to is in the middle of a line, then the subsequent line reported may be incomplete (missing the part of the line before the position it was advanced to).

---

_Label `enhancement` added by @BurntSushi on 2020-06-15 11:34_

---
