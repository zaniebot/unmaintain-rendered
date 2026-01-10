```yaml
number: 19377
title: "Move grouped rendering to `ruff_db`"
type: pull_request
state: closed
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
draft: true
base: main
head: brent/grouped
created_at: 2025-07-15T21:28:59Z
updated_at: 2025-07-24T13:31:53Z
url: https://github.com/astral-sh/ruff/pull/19377
synced_at: 2026-01-10T17:58:13Z
```

# Move grouped rendering to `ruff_db`

---

_Pull request opened by @ntBre on 2025-07-15 21:28_

Summary
--

This is a pretty large refactor. The `grouped` output format depends on code from the more general `TextEmitter`, so in addition to moving over the `grouped` module itself, I also copied the necessary supporting code from `ruff_linter::message::text` and `ruff_linter::message::diff`. The `text` module also depended on the `ruff_linter::line_width` module, so I moved that to `ruff_db` too. Similarly, I moved `Locator::ceil_char_boundary` to a free function in `ruff_db`, along with a TODO to make it private once the `text` module is moved over.

Test Plan
--

Existing tests ported to `ruff_db`, plus a new test to see what a missing file looks like in this format

---

_Label `internal` added by @ntBre on 2025-07-15 21:29_

---

_Label `diagnostics` added by @ntBre on 2025-07-15 21:29_

---

_Renamed from "Move grouped rendering to ruff_db" to "Move grouped rendering to `ruff_db`" by @ntBre on 2025-07-15 21:29_

---

_Comment by @github-actions[bot] on 2025-07-15 21:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-15 21:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_db/Cargo.toml`:29 on 2025-07-16 08:09_

Can we use `anstyle` instead of `colored`? both crates do the same

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:878 on 2025-07-16 08:12_

Having this type in `render` feels wrong to me. It is very specific to ruff's linter and may not apply to ty. 



---

_@MichaReiser reviewed on 2025-07-16 08:16_

Looking at this. It's not clear to me what the benefit is of using this type to `ruff_db`. 

That's probably a question I should have asked earlier but what's the motivation for moving all rendering code to `ruff_db`? 

IMO, the only reason for moving any rendering code to `ruff_db` is if it allows reuse between ruff and ty and we should, therefore, only move output formats that we intend on sharing between ruff and ty to `ruff_db`. All other output formats can remain in ruff. 

I think for now, the focus should be on whether we can reuse concise and full in Ruff because it would allow us to share more code between the two crates (and diagnostics rendering is fairly involved). Moving all other output formats seems less of a priority to me unless we want to use them in ty.

That's why I think we shouldn't move grouped just yet

---

_Comment by @ntBre on 2025-07-24 13:31_

Closing this for now, it would be totally different after working on `concise` and `full`, if we even decide to move it over.

---

_Closed by @ntBre on 2025-07-24 13:31_

---

_Branch deleted on 2025-07-24 13:31_

---
