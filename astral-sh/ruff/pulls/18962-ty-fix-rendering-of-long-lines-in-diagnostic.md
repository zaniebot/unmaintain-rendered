```yaml
number: 18962
title: "[ty] Fix rendering of long lines in diagnostic messages that are indented with tabs"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/fix-diagnostic-rendering-panic
created_at: 2025-06-26T14:55:54Z
updated_at: 2025-06-26T15:12:52Z
url: https://github.com/astral-sh/ruff/pull/18962
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Fix rendering of long lines in diagnostic messages that are indented with tabs

---

_Pull request opened by @BurntSushi on 2025-06-26 14:55_

It turns out that `annotate-snippets` doesn't do a great job of
consistently handling tabs. The intent of the implementation is clearly
to expand tabs into 4 ASCII whitespace characters. But there are a few
places where the column computation wasn't taking this expansion into
account. In particular, the `unicode-width` crate returns `None` for a
`\t` input, and `annotate-snippets` would in turn treat this as either
zero columns or one column. Both are wrong.

In patching this, it caused one of the existing `annotate-snippets`
tests to fail. I spent a fair bit of time on it trying to fix it before
coming to the conclusion that the test itself was wrong. In particular,
the annotation ranges are 4 bytes off. However, when the range was
wrong, the buggy code was rendering the example as intended since
`\t` characters were treated as taking up zero columns of space. Now
that they are correctly computed as taking up 4 columns of space, the
offsets of the test needed to be adjusted.

Fixes astral-sh/ty#670


---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-26 14:56_

---

_Label `bug` added by @BurntSushi on 2025-06-26 14:56_

---

_Label `ty` added by @BurntSushi on 2025-06-26 14:56_

---

_Label `diagnostics` added by @BurntSushi on 2025-06-26 14:56_

---

_Comment by @MichaReiser on 2025-06-26 14:58_

> In particular, the unicode-width crate returns None for a `\t` input

That's a somewhat "recent" change in unicode-width, I think it used to return 1. So maybe that used to work

---

_@MichaReiser approved on 2025-06-26 14:58_

---

_Comment by @MichaReiser on 2025-06-26 14:59_

Uff, that sounds awful to debug. Thanks for tracking this down

---

_Comment by @github-actions[bot] on 2025-06-26 15:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @BurntSushi on 2025-06-26 15:12_

> > In particular, the unicode-width crate returns None for a `\t` input
> 
> That's a somewhat "recent" change in unicode-width, I think it used to return 1. So maybe that used to work

I think this was for `\n`, not `\t`: https://github.com/unicode-rs/unicode-width/issues/60

---

_Merged by @BurntSushi on 2025-06-26 15:12_

---

_Closed by @BurntSushi on 2025-06-26 15:12_

---

_Branch deleted on 2025-06-26 15:12_

---
