```yaml
number: 2519
title: "Dropping redundant re-exports from completions might consider current module (or whether it's in first party code)"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2026-01-15T15:18:53Z
updated_at: 2026-01-15T15:18:53Z
url: https://github.com/astral-sh/ty/issues/2519
synced_at: 2026-01-15T15:50:04Z
```

# Dropping redundant re-exports from completions might consider current module (or whether it's in first party code)

---

_@BurntSushi_

With https://github.com/astral-sh/ruff/pull/22581, ty started filtering out completions for redundant re-exports deeper in a package's hierarchy. For example, instead of suggesting all of `pandas.read_csv`, `pandas.io.api.read_csv`, `pandas.io.parsers.read_csv` and `pandas.io.parsers.readers.read_csv`, ty will only suggest `pandas.read_csv`.

However, if you're _working on_ Pandas, you might reasonably want to see all available re-exports. Or perhaps even more precisely, see the "closest" re-export. The latter is perhaps ideal, but requires defining a notion of "closesness" that is more refind than "top-most." We could start by implementing the former and leave the latter for future work.

That is, it should be pretty straight-forward to avoid doing any kind of dropping of re-exports when you're within first party code (just look at the search path of the containing `Module`). So that if you're working on pandas, you'll see all 4 re-exports. But if you're depending on pandas, you only get the top-level one.

The one thorny part of this though is testing. We don't have a great story right now for testing how we handle Python files coming from different search paths in a granular way. We could add support to it in our `CursorTest` infrastructure. An alternative is to write an end-to-end integration test.

---

_Added to milestone `Stable` by @BurntSushi on 2026-01-15 15:18_

---

_Label `server` added by @BurntSushi on 2026-01-15 15:18_

---

_Label `completions` added by @BurntSushi on 2026-01-15 15:18_

---
