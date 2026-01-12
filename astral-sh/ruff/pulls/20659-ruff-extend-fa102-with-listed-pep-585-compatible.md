```yaml
number: 20659
title: "[`ruff`] Extend FA102 with listed PEP 585-compatible APIs"
type: pull_request
state: merged
author: terror
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-fa102-collections
created_at: 2025-10-01T02:41:00Z
updated_at: 2025-10-03T16:20:49Z
url: https://github.com/astral-sh/ruff/pull/20659
synced_at: 2026-01-12T15:57:07Z
```

# [`ruff`] Extend FA102 with listed PEP 585-compatible APIs

---

_@terror_

Resolves https://github.com/astral-sh/ruff/issues/20512

This PR expands FA102’s preview coverage to flag every PEP 585-compatible API that breaks without from `from __future__ import annotations`, including `collections.abc`. The rule now treats asyncio futures, pathlib-style queues, weakref containers, shelve proxies, and the full `collections.abc` family as generics once preview mode is enabled. 

Stable behavior is unchanged; the broader matching runs behind `is_future_required_preview_generics_enabled`, letting us vet the new diagnostics before marking them as stable. 

I've also added a snapshot test that covers all of the newly supported  types.

Check out https://docs.python.org/3/library/stdtypes.html#standard-generic-classes for a list of commonly used PEP 585-compatible APIs.

---

_Marked ready for review by @terror on 2025-10-01 02:47_

---

_Renamed from "Extend FA102 with `collections.abc` types" to "[`ruff`] Extend FA102 with `collections.abc` types" by @terror on 2025-10-01 17:20_

---

_@amyreese approved on 2025-10-02 00:23_

---

_Review requested from @ntBre by @amyreese on 2025-10-02 00:23_

---

_Comment by @github-actions[bot] on 2025-10-02 00:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/preview.rs`:203 on 2025-10-02 20:36_

nit: this comment is generally the PR link. The description seems nice to have too, but the link is handy when working on stabilizations.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_future_annotations/mod.rs`:69 on 2025-10-02 20:37_

We usually add a second test function like `fa102_preview` and only enable preview there. That way we can see the diff between the preview and non-preview behavior. This case is pretty straightforward, but I'd still prefer that.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:159 on 2025-10-02 20:52_

I think it might make sense to inline `has_pep_585_generic` here and remove its comment about `as_pep_585_generic`. Unless I'm missing something, this is now the only reference to `has_pep_585_generic`.

I also found a few issues related to some of the other PEP 585 helpers here, but they seem unrelated to FA102. It looks like two of the PEP 585 helpers are only used by FA102 and two only used by UP006.

- https://github.com/astral-sh/ruff/pull/5454
- https://github.com/astral-sh/ruff/pull/15250
- https://github.com/astral-sh/ruff/issues/15251

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:168 on 2025-10-02 20:54_

Should we go ahead and add the other [PEP 585](https://peps.python.org/pep-0585/#implementation)/[standard generic aliases](https://docs.python.org/3/library/stdtypes.html#standard-generic-classes) while we're here?

That's actually how I found the related issues above, but it seems safer to do here than in the UP rule helpers.

---

_@ntBre reviewed on 2025-10-02 20:55_

Nice! This looks good to me overall. I just had a couple of small nits and one question/suggestion to handle other PEP-585 generics while we're here. But I'm curious to hear your and @amyreese's thoughts on that too.

---

_Label `rule` added by @ntBre on 2025-10-02 20:56_

---

_Label `preview` added by @ntBre on 2025-10-02 20:56_

---

_@terror reviewed on 2025-10-02 20:58_

---

_Review comment by @terror on `crates/ruff_linter/src/preview.rs`:203 on 2025-10-02 20:58_

No worries, I'll switch that over!

---

_@terror reviewed on 2025-10-02 21:03_

---

_Review comment by @terror on `crates/ruff_python_semantic/src/analyze/typing.rs`:159 on 2025-10-02 21:03_

Yeah I was wondering why that was separated, I'll go ahead and inline it.

---

_@terror reviewed on 2025-10-02 21:04_

---

_Review comment by @terror on `crates/ruff_python_semantic/src/analyze/typing.rs`:168 on 2025-10-02 21:04_

Yep that makes sense to me, better to catch all cases here than having another extension to this rule later on.

---

_Renamed from "[`ruff`] Extend FA102 with `collections.abc` types" to "[`ruff`] Extend FA102 with rest of PEP 585 generics" by @terror on 2025-10-02 21:23_

---

_Renamed from "[`ruff`] Extend FA102 with rest of PEP 585 generics" to "[`ruff`] Extend FA102 with rest of PEP 585-compatible APIs" by @terror on 2025-10-02 21:28_

---

_Comment by @terror on 2025-10-02 21:39_

@ntBre @amyreese I've re-purposed this PR to include listed PEP 585-compatible APIs [here](https://docs.python.org/3/library/stdtypes.html#standard-generic-classes). I've added a preview/non-preview test for all of these as well. 

---

_Renamed from "[`ruff`] Extend FA102 with rest of PEP 585-compatible APIs" to "[`ruff`] Extend FA102 with listed PEP 585-compatible APIs" by @terror on 2025-10-02 21:40_

---

_@amyreese approved on 2025-10-03 00:26_

---

_@ntBre approved on 2025-10-03 13:45_

Very nice, thank you!

---

_Merged by @ntBre on 2025-10-03 13:45_

---

_Closed by @ntBre on 2025-10-03 13:45_

---

_Branch deleted on 2025-10-03 16:20_

---
