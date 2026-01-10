```yaml
number: 895
title: "Explore reconciling `SymbolVisitor` with `exported_symbols`"
type: issue
state: closed
author: UnboundVariable
labels: []
assignees: []
created_at: 2025-07-25T20:16:31Z
updated_at: 2025-12-04T20:27:49Z
url: https://github.com/astral-sh/ty/issues/895
synced_at: 2026-01-10T01:56:40Z
```

# Explore reconciling `SymbolVisitor` with `exported_symbols`

---

_Issue opened by @UnboundVariable on 2025-07-25 20:16_

[PR #19521](https://github.com/astral-sh/ruff/pull/19521) implements a new `SymbolVisitor` that collects symbols for the "workspace symbols" and "document symbols" language server features. The same visitor mechanism is designed to be usable by a future auto-import feature as well.

As part of the [code review for this PR](https://github.com/astral-sh/ruff/pull/19521#discussion_r2228057233), it was pointed out that the `SymbolVisitor` functionality overlaps that of the existing `exported_names` call. While there is significant overlap, there are also some differences noted in the code review discussion.

It's not clear that the `exported_names` feature could be used to implement auto-import. It can probably be made to support "document symbols" and "workspace symbols", but some changes (perhaps mode flags) would be required. 

---

_Closed by @BurntSushi on 2025-12-04 18:21_

---

_Comment by @AlexWaygood on 2025-12-04 18:24_

(Closed in https://github.com/astral-sh/ruff/pull/21779 because `SymbolVisitor` is now substantially different to `ExportFinder`, and will probably become more so in the future, so the answer now seems clear that we shouldn't try to unify the two.)

---

_Reopened by @carljm on 2025-12-04 20:27_

---

_Closed by @carljm on 2025-12-04 20:27_

---
