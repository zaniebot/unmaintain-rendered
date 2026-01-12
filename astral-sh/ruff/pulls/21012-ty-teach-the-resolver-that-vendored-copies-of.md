```yaml
number: 21012
title: "[ty] Teach the resolver that vendored copies of typeshed in the ty cache are in fact typeshed"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
base: main
head: gankra/goto-typeshed
created_at: 2025-10-21T04:11:03Z
updated_at: 2025-10-21T17:57:03Z
url: https://github.com/astral-sh/ruff/pull/21012
synced_at: 2026-01-12T15:57:14Z
```

# [ty] Teach the resolver that vendored copies of typeshed in the ty cache are in fact typeshed

---

_@Gankra_

Fixes https://github.com/astral-sh/ty/issues/1054

I've confirmed it works on my machine in vscode, but it's not clear to me if there's a good way to test this in our test suite.

---

_Label `server` added by @Gankra on 2025-10-21 04:11_

---

_Review requested from @carljm by @Gankra on 2025-10-21 04:11_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-21 04:11_

---

_Label `ty` added by @Gankra on 2025-10-21 04:11_

---

_Review requested from @sharkdp by @Gankra on 2025-10-21 04:11_

---

_Review requested from @dcreager by @Gankra on 2025-10-21 04:11_

---

_Review requested from @MichaReiser by @Gankra on 2025-10-21 04:11_

---

_@Gankra reviewed on 2025-10-21 04:11_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/module_resolver/path.rs`:751 on 2025-10-21 04:11_

We invoke these *a lot* now, so they might need to be cached...

---

_Comment by @github-actions[bot] on 2025-10-21 04:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-21 04:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/path.rs`:737 on 2025-10-21 06:19_

Hmm. I'd prefer if we can keep this logic in `ty_server`, if possible because not all clients have a cache location or they use a different cache location (e.g. playground)

---

_@MichaReiser reviewed on 2025-10-21 06:23_

Could we achive the same if the LSP registered an additional search path for the cached vendored files (that comes after the standard library?) That would require less custom casing for which we have not tests for. 

Another alternative would be to remap cached vendored file path to  the corresponding vendored path within the LSP request handlers. 

Could any of those solution lead to problems if the content of the cached vendored file and the vendored file diverge (e.g. because the user starts to make edits?)

---

_Renamed from "Teach the resolver that vendored copies of typeshed in the ty cache are in fact typeshed" to "[ty] Teach the resolver that vendored copies of typeshed in the ty cache are in fact typeshed" by @AlexWaygood on 2025-10-21 06:37_

---

_Comment by @Gankra on 2025-10-21 13:53_

> Could we achive the same if the LSP registered an additional search path for the cached vendored files (that comes after the standard library?) That would require less custom casing for which we have not tests for.

The additional search-path still requires something like `path_is_equivalent` logic to not get rejected on the `file.path() == module.file().path()` logic. I'd also be worried that "the stdlib can't be shadowed" rules might get in the way? But probably it would do the right thing for this code specifically so maybe it's fine?

> Another alternative would be to remap cached vendored file path to the corresponding vendored path within the LSP request handlers.

Hmm that was concerning to me as an approach earlier but maybe it's not actually that bad? We already do that mapping in the other direction. I'll give it a try.

---

_Closed by @Gankra on 2025-10-21 17:57_

---
