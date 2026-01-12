```yaml
number: 19977
title: "Replace `pyflakes` hand-crafted tests with inline snapshots"
type: pull_request
state: closed
author: dylwil3
labels:
  - internal
  - testing
assignees: []
draft: true
base: main
head: pyflakes-tests
created_at: 2025-08-18T21:11:12Z
updated_at: 2025-12-10T17:59:20Z
url: https://github.com/astral-sh/ruff/pull/19977
synced_at: 2026-01-12T15:56:51Z
```

# Replace `pyflakes` hand-crafted tests with inline snapshots

---

_@dylwil3_

This PR makes an incremental improvement on the maintainability of our `pyflakes` test fixtures by replacing hundreds of hand-written tests of the form

```rust
assert_eq!(actual,expected)
```

with inline snapshot tests.

There are two commits - the first keeps the same format, pretty much, as the original tests. It's compact but a little hard to review the snapshots out of context. The second has snippets of our diagnostic output. Let me know if you'd like me to experiment with yet another view.


---

_Label `internal` added by @dylwil3 on 2025-08-18 21:11_

---

_Label `testing` added by @dylwil3 on 2025-08-18 21:11_

---

_Comment by @github-actions[bot] on 2025-08-18 21:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @dylwil3 on 2025-08-18 21:32_

---

_Comment by @MichaReiser on 2025-08-19 12:49_

I haven't looked at the details but I'd prefer if we can avoid a 11k file. IDEs struggle with files that large (rustfmt as well), I do, and LLMs do as well :)

---

_Comment by @dylwil3 on 2025-08-19 12:54_

> I haven't looked at the details but I'd prefer if we can avoid a 11k file. IDEs struggle with files that large (rustfmt as well), I do, and LLMs do as well :)

Ok I reverted to the more concise output. I guess the alternative would be to move these tests to separate files and use ordinary, non-inline snapshots. It's already quite a large file, so maybe that's a good idea?

---

_Comment by @ntBre on 2025-09-10 14:13_

I'd probably go for a bunch of new snapshot files over the inline snapshots. But otherwise I liked the version with `test_snippet` and `assert_diagnostics`!

---

_Comment by @MichaReiser on 2025-12-10 17:42_

@dylwil3 is this something you still plan on doing or should we close the PR?

---

_Closed by @dylwil3 on 2025-12-10 17:59_

---
