```yaml
number: 998
title: Workspace symbols is very slow
type: issue
state: closed
author: MichaReiser
labels:
  - performance
  - server
assignees: []
created_at: 2025-08-15T07:23:33Z
updated_at: 2025-08-23T16:53:42Z
url: https://github.com/astral-sh/ty/issues/998
synced_at: 2026-01-10T02:06:24Z
```

# Workspace symbols is very slow

---

_Issue opened by @MichaReiser on 2025-08-15 07:23_

Searching for workspace symbols can be very slow in large projects. We should add some form of caching to workspace symbols to traversing every single file on every request. 

I don't think it's necessary to build a full search index. Using only salsa for caching should probably be fine for an initial version:

* Make [`symbols_for_file`](https://github.com/astral-sh/ruff/blob/a0d8ff51dddd7c4d524323d4a8476f3b62e7681f/crates/ty_ide/src/symbols.rs#L74-L90) a salsa query that doesn't take any option and returns all symbols of that file. We could consider making it two queries: One that returns hierarchical and another that returns non-hierarchical symbol information. The most important part is that the query isn't parametrized by the query string. Instead, filtering should happen after collecting all symbols. This gives us per-file caching, which should make subsequent lookups very cheap
* Iterate over all files in `workspace_symbols` and apply the filtering based on the user input

---

_Label `performance` added by @MichaReiser on 2025-08-15 07:23_

---

_Label `server` added by @MichaReiser on 2025-08-15 07:23_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-15 07:23_

---

_Comment by @MichaReiser on 2025-08-15 07:24_

CC: @BurntSushi, because an alternative is to reuse the index for auto import. But maybe this form of caching would be a good start for auto import too and we can build anything more fancy if the need arises in the future.

---

_Assigned to @BurntSushi by @BurntSushi on 2025-08-15 11:51_

---

_Removed from milestone `GA` by @MichaReiser on 2025-08-15 12:21_

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:21_

---

_Closed by @BurntSushi on 2025-08-23 16:53_

---
