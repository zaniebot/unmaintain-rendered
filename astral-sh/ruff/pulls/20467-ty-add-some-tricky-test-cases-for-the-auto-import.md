```yaml
number: 20467
title: "[ty] Add some tricky test cases for the auto-import importer"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - testing
  - ty
assignees: []
merged: true
base: main
head: ag/tricky-importer-tests
created_at: 2025-09-18T13:11:45Z
updated_at: 2025-09-19T11:54:09Z
url: https://github.com/astral-sh/ruff/pull/20467
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Add some tricky test cases for the auto-import importer

---

_Pull request opened by @BurntSushi on 2025-09-18 13:11_

We don't attempt to fix these yet. I think there are bigger fish to fry.

I came up with these based on this discussion:
https://github.com/astral-sh/ruff/pull/20439#discussion_r2357769518

Here's one example:

```
if ...:
    from foo import MAGIC
else:
    from bar import MAGIC

MAG<CURSOR>
```

Now in this example, completions will include `MAGIC` from the local
scope. That is, auto-import _shouldn't_ really be involved with completions. But at
present, auto-import will suggest importing `foo` and `bar` because we
haven't de-duplicated completions yet. Which is fine.

Here's another example:

```
if ...:
    import foo as fubar
else:
    import bar as fubar

MAG<CURSOR>
```

Now here, there is no `MAGIC` symbol in scope. So auto-import is in
play. Let's assume that the user selects `MAGIC` from `foo` in this
example. (`bar` also has `MAGIC`.)

Since we currently ignore the declaration site for symbols with
multiple possible bindings, the importer today doesn't know that
`fubar` _could_ contain `MAGIC`. But even if it did, what would we do
with that information? Should we do this?

```
if ...:
    import foo as fubar
    from foo import MAGIC
else:
    import bar as fubar

MAGIC
```

Or could we reason that `bar` also has `MAGIC`?

```
if ...:
    import foo as fubar
else:
    import bar as fubar

fubar.MAGIC
```

But if we did that, we're making an assumption of user intent, since
they *selected* `foo.MAGIC` but not `bar.MAGIC`.

Anyway, I don't think we need to settle on an answer today, but I
wanted to capture some of these tricky cases in tests at the very
least.


---

_Review requested from @carljm by @BurntSushi on 2025-09-18 13:11_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-09-18 13:11_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-09-18 13:11_

---

_Review requested from @sharkdp by @BurntSushi on 2025-09-18 13:11_

---

_Review requested from @dcreager by @BurntSushi on 2025-09-18 13:11_

---

_Review request for @dcreager removed by @BurntSushi on 2025-09-18 13:12_

---

_Review request for @carljm removed by @BurntSushi on 2025-09-18 13:12_

---

_Comment by @github-actions[bot] on 2025-09-18 13:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-18 13:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @BurntSushi on 2025-09-18 13:30_

---

_Label `server` added by @BurntSushi on 2025-09-18 13:30_

---

_Label `ty` added by @BurntSushi on 2025-09-18 13:30_

---

_Label `internal` removed by @MichaReiser on 2025-09-19 07:42_

---

_Label `testing` added by @MichaReiser on 2025-09-19 07:42_

---

_@MichaReiser approved on 2025-09-19 07:43_

---

_Merged by @BurntSushi on 2025-09-19 11:54_

---

_Closed by @BurntSushi on 2025-09-19 11:54_

---

_Branch deleted on 2025-09-19 11:54_

---
