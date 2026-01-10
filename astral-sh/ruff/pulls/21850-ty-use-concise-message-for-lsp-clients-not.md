```yaml
number: 21850
title: "[ty] Use concise message for LSP clients not supporting related diagnostic information"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-concise-output
created_at: 2025-12-08T16:21:30Z
updated_at: 2025-12-09T12:18:32Z
url: https://github.com/astral-sh/ruff/pull/21850
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Use concise message for LSP clients not supporting related diagnostic information

---

_Pull request opened by @MichaReiser on 2025-12-08 16:21_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1582

Use the concise message if the client doesn't support related information. 
Otherwise use the primary message and primary annotation message (if available).

## Test Plan

Added e2e tests


---

_Review requested from @carljm by @MichaReiser on 2025-12-08 16:21_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-08 16:21_

---

_Label `server` added by @MichaReiser on 2025-12-08 16:21_

---

_Label `ty` added by @MichaReiser on 2025-12-08 16:21_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-08 16:21_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 16:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 16:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 39 diagnostics
+ Found 38 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5544 diagnostics
+ Found 5543 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2025-12-08 22:13_

Seems reasonable enough

---

_Comment by @MichaReiser on 2025-12-09 01:42_

I think I now lean towards always using the concise message because the vertical space is fairly limited, similar to the CLI. This should also simplify the implemnentation a tiny bit

---

_Merged by @MichaReiser on 2025-12-09 12:18_

---

_Closed by @MichaReiser on 2025-12-09 12:18_

---

_Branch deleted on 2025-12-09 12:18_

---
