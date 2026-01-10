```yaml
number: 21302
title: "chore: bump dist, remove old commenting workflows"
type: pull_request
state: merged
author: woodruffw
labels:
  - ci
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ww/bump-dist
created_at: 2025-11-06T18:03:13Z
updated_at: 2025-11-11T18:48:15Z
url: https://github.com/astral-sh/ruff/pull/21302
synced_at: 2026-01-10T16:53:55Z
```

# chore: bump dist, remove old commenting workflows

---

_Pull request opened by @woodruffw on 2025-11-06 18:03_

This removes the four `workflow_run`-based commenting workflows now that astral-sh-bot can track `workflow_run` events.


---

_Closed by @woodruffw on 2025-11-06 18:10_

---

_Reopened by @woodruffw on 2025-11-06 18:10_

---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-11-06 19:30_

---

_Label `ecosystem-analyzer` removed by @woodruffw on 2025-11-07 15:21_

---

_Comment by @github-actions[bot] on 2025-11-07 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @astral-sh-bot[bot] on 2025-11-07 17:23_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @github-actions[bot] on 2025-11-07 17:34_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-07 17:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @astral-sh-bot[bot] on 2025-11-07 18:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-07 18:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `ecosystem-analyzer` added by @woodruffw on 2025-11-07 18:54_

---

_Comment by @astral-sh-bot[bot] on 2025-11-07 18:59_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://ww-bump-dist.ecosystem-663.pages.dev/diff)** ([timing results](https://ww-bump-dist.ecosystem-663.pages.dev/timing))




---

_Comment by @github-actions[bot] on 2025-11-07 18:59_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://ww-bump-dist.ecosystem-663.pages.dev/diff)** ([timing results](https://ww-bump-dist.ecosystem-663.pages.dev/timing))


---

_Renamed from "chore: bump cargo dist" to "chore: bump dist, remove old commenting workflows" by @woodruffw on 2025-11-07 19:11_

---

_Label `ci` added by @woodruffw on 2025-11-07 19:15_

---

_Marked ready for review by @woodruffw on 2025-11-07 19:22_

---

_Review requested from @MichaReiser by @woodruffw on 2025-11-07 19:22_

---

_Review requested from @AlexWaygood by @woodruffw on 2025-11-07 19:22_

---

_Review requested from @sharkdp by @woodruffw on 2025-11-07 19:22_

---

_@MichaReiser reviewed on 2025-11-07 19:37_

Thank you. 



---

_@sharkdp approved on 2025-11-07 22:00_

Thank you!

---

_Comment by @woodruffw on 2025-11-07 22:08_

Thanks both! I'll land this in a moment, but it can be reverted if it causes any disruption.



---

_Merged by @woodruffw on 2025-11-07 22:09_

---

_Closed by @woodruffw on 2025-11-07 22:09_

---

_Branch deleted on 2025-11-07 22:09_

---

_Comment by @UnknownPlatypus on 2025-11-08 18:35_

Out of curiosity, where can I see how the bot is setup ?
I would also like to drop these commenting workflows in my projects

---

_Comment by @woodruffw on 2025-11-08 18:55_

> Out of curiosity, where can I see how the bot is setup ?

It's not public at the moment, but we may consider making it public once it's bit more mature 🙂

For reference however, it's very similar to CPython's "bedevere" bot: https://github.com/python/bedevere

Mariatta Wijaya has a really great guide on building these, which our bot also follows pretty closely: https://github-app-tutorial.readthedocs.io/en/latest/creating-github-app.html

---

_Comment by @UnknownPlatypus on 2025-11-11 18:48_

Awesome, very cool. Thanks for the insights, this is very valuable :raised_hands:  

---
