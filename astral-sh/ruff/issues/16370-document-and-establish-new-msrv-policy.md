```yaml
number: 16370
title: Document and establish new MSRV policy
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-02-25T13:04:32Z
updated_at: 2025-03-13T03:17:09Z
url: https://github.com/astral-sh/ruff/issues/16370
synced_at: 2026-01-12T15:54:55Z
```

# Document and establish new MSRV policy

---

_@MichaReiser_

* [x] Update Ruff's versioning policy to allow MSRV bumps in patch versions
* [x] Decide on whether we want to use latest - 1 or latest - 2 (future bumps should happen as part or shortly after bumping the rust toolchain version)
* [x] Bump the MSRV accordingly


---

_Label `internal` added by @MichaReiser on 2025-02-25 13:04_

---

_Comment by @MichaReiser on 2025-02-25 13:05_

@carljm do you want to pick this up as you seem most excited about a more recent MSRV?

---

_Comment by @carljm on 2025-02-25 16:55_

Sure, I can do this. I checked off the "decide on N-1 vs N-2" task above, since I think we've agreed on N-2.

---

_Comment by @charliermarsh on 2025-03-13 00:53_

I think this got done in the linked PRs?

---

_Closed by @charliermarsh on 2025-03-13 00:53_

---
