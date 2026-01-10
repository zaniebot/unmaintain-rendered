```yaml
number: 21241
title: "[ty] Add `ty_server::Db` trait"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/server-db-trait
created_at: 2025-11-03T03:01:00Z
updated_at: 2025-11-05T13:44:21Z
url: https://github.com/astral-sh/ruff/pull/21241
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Add `ty_server::Db` trait

---

_Pull request opened by @MichaReiser on 2025-11-03 03:01_

## Summary

For notebooks, we'll need a way to query the metadata of an arbitrary notebook when mapping 
e.g. the range of a find reference operation to the reference's location. 

This PR introduces a new `Db` trait and implements it for `ProjectDatabase`. 
The new `Db` trait allows quering the document metadata by path, assuming
that the `ProjectDatabase` uses the `LSPSystem` (which is always true). 



---

_Label `internal` added by @MichaReiser on 2025-11-03 03:01_

---

_Label `server` added by @MichaReiser on 2025-11-03 03:01_

---

_Label `ty` added by @MichaReiser on 2025-11-03 03:01_

---

_Review requested from @carljm by @MichaReiser on 2025-11-03 03:01_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-03 03:01_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-03 03:01_

---

_@MichaReiser reviewed on 2025-11-03 03:02_

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:453 on 2025-11-03 03:02_

I had to change the signature of `ProgressReporter` to take a `ProjectDatabase` as argument because we now need a `ty_server::Db` instead of a `ty_project::Db` in the LSP.

---

_Comment by @github-actions[bot] on 2025-11-03 03:07_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 03:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-11-03 03:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fserver-db-trait?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21241 will **not alter performance**

<sub>Comparing <code>micha/server-db-trait</code> (71115b3) with <code>main</code> (7009d60)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Fserver-db-trait?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Review comment by @sharkdp on `crates/ty_server/src/db.rs`:9 on 2025-11-03 15:26_

I find this name shadowing here confusing. Is there another name that we could use for the trait here? I assume you named it `Db` to avoid all kinds of downstream changes?

---

_Review comment by @sharkdp on `crates/ty_server/src/db.rs`:12 on 2025-11-03 15:27_

Maybe
```suggestion
    fn document_from_file(&self, file: File) -> Option<&Document>;
```

and similar for `notebook_document` below?

---

_@sharkdp approved on 2025-11-03 15:27_

---

_@MichaReiser reviewed on 2025-11-03 15:30_

---

_Review comment by @MichaReiser on `crates/ty_server/src/db.rs`:9 on 2025-11-03 15:30_

The idea is that you can always import `Db` without having to think within which crate you're in (this is also how all our other `Db` traits are defined)

---

_@sharkdp reviewed on 2025-11-03 15:33_

---

_Review comment by @sharkdp on `crates/ty_server/src/db.rs`:9 on 2025-11-03 15:33_

Makes sense, thank you.

---

_@MichaReiser reviewed on 2025-11-05 13:29_

---

_Review comment by @MichaReiser on `crates/ty_server/src/db.rs`:12 on 2025-11-05 13:29_

I find `from` more confusing because `From` implies that there's a conversion at play. But that's not the case. The operation here is closer to a `map.get(file)` where `map` is a `HashMap<File, Document>`. 



---

_Merged by @MichaReiser on 2025-11-05 13:40_

---

_Closed by @MichaReiser on 2025-11-05 13:40_

---

_Branch deleted on 2025-11-05 13:40_

---
