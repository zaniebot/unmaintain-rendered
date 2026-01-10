```yaml
number: 20911
title: Avoid printing out entire search path if long
type: pull_request
state: closed
author: hauntsaninja
labels: []
assignees: []
base: main
head: searchpathlen
created_at: 2025-10-16T06:42:27Z
updated_at: 2025-10-16T07:58:20Z
url: https://github.com/astral-sh/ruff/pull/20911
synced_at: 2026-01-10T17:34:34Z
```

# Avoid printing out entire search path if long

---

_Pull request opened by @hauntsaninja on 2025-10-16 06:42_

We have environments with a thousand entries in our search path.
This overwhelms ty output

---

_Review requested from @carljm by @hauntsaninja on 2025-10-16 06:42_

---

_Review requested from @AlexWaygood by @hauntsaninja on 2025-10-16 06:42_

---

_Review requested from @sharkdp by @hauntsaninja on 2025-10-16 06:42_

---

_Review requested from @dcreager by @hauntsaninja on 2025-10-16 06:42_

---

_Comment by @github-actions[bot] on 2025-10-16 06:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 06:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-10-16 07:18_

What a coincidence that we worked on this at the exact same time... https://github.com/astral-sh/ruff/pull/20912

I'll close this as my PR retains the option to list all search paths by opting in with `-v`.

---

_Closed by @MichaReiser on 2025-10-16 07:19_

---

_Comment by @hauntsaninja on 2025-10-16 07:58_

Oh funny! I didn't even see the issue in the ty repo, was just testing out a22 :-) Thanks!

---
