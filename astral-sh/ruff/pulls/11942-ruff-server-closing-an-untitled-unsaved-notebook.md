```yaml
number: 11942
title: "`ruff server`: Closing an untitled, unsaved notebook document no longer throws an error"
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/fix-untitled-notebook-closing
created_at: 2024-06-19T18:49:38Z
updated_at: 2024-06-21T19:33:22Z
url: https://github.com/astral-sh/ruff/pull/11942
synced_at: 2026-01-10T21:56:00Z
```

# `ruff server`: Closing an untitled, unsaved notebook document no longer throws an error

---

_Pull request opened by @snowsignal on 2024-06-19 18:49_

## Summary

Fixes #11651.
Fixes #11851.

We were double-closing a notebook document from the index, once in `textDocument/didClose` and then in the `notebookDocument/didClose` handler. The second time this happens, taking a snapshot fails.

I've rewritten how we handle snapshots for closing notebooks / notebook cells so that any failure is simply logged instead of propagating upwards. This implementation works consistently even if we don't receive `textDocument/didClose` notifications for each specific cell, since they get closed (and the diagnostics get cleared) in the notebook document removal process.

## Test Plan

1. Open an untitled, unsaved notebook with the `Create: New Jupyter Notebook` command from the VS Code command palette (`Ctrl/Cmd + Shift + P`)
2. Without saving the document, close it.
3. No error popup should appear.
4. Run the debug command (`Ruff: print debug information`) to confirm that there are no open documents


---

_Label `bug` added by @snowsignal on 2024-06-19 18:49_

---

_Label `server` added by @snowsignal on 2024-06-19 18:49_

---

_Added to milestone `Ruff Server: Stable` by @snowsignal on 2024-06-19 18:49_

---

_Comment by @github-actions[bot] on 2024-06-19 19:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @snowsignal on 2024-06-20 02:10_

I've confirmed that this also fixes #11851 😄 

---

_@MichaReiser approved on 2024-06-20 06:37_

Unfortunately, the LSP specification isn't explicit about whether the document-specific `did*` notifications are sent for notebook documents. It might be worth opening an issue on the LSP spec repository to ask for clarification. 

This is the sort of change where I would love to have the ability to write E2E tests, ideally with a way to debug if the document was indeed closed (e.g. by using the debug command?)

Would you mind updating your test plan with a verification that the document actually was removed and that we aren't just leaking memory now (e.g. by running the debug command after closing all documents and checking that the open document count is now down to zero (including cells)

---

_Comment by @snowsignal on 2024-06-21 05:16_

@MichaReiser As it turns out, there was a bug in this PR that would leave notebook documents remaining in the index. I've re-done this PR in a way that avoids assumptions about call order and makes snapshot failures non-failing.

---

_Review requested from @MichaReiser by @snowsignal on 2024-06-21 05:17_

---

_Comment by @MichaReiser on 2024-06-21 06:04_

@snowsignal I'm okay with the change itself but I get the impression that we don't fully understand the lifecycle events of notebook documents (at least I don't!) 

I think it would be very valuable to document what the lifecycle events for notebooks are. Why do we need to deregister them in both places? Isn't just one enough. I'm okay merging this PR as a fix but we should follow up by either figuring the behavior out ourselves or creating an issue on the LSP project to ask for clarification (or study e.g. the VS Code source code)

---

_Comment by @codspeed-hq[bot] on 2024-06-21 17:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/fix-untitled-notebook-closing)

### Merging #11942 will **improve performances by 5.07%**

<sub>Comparing <code>jane/server/fix-untitled-notebook-closing</code> (858d5c6) with <code>main</code> (3d0230f)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jane/server/fix-untitled-notebook-closing` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 1.9 ms | 1.8 ms | +5.07% |


---

_Comment by @snowsignal on 2024-06-21 17:53_

@MichaReiser After considering this further, I removed the redundant de-registration loop from `close_document`, and confirmed that it still works.

How would you want me to go about documenting the notebook cell lifecycle? I've documented how notebook cells are removed via `textDocument/didClose` here: https://github.com/astral-sh/ruff/blob/3d0230f46958a7821ac64493303e3c6df61153b4/crates/ruff_server/src/session/index.rs#L340-L344

What do you think is missing?

I'll add that documentation in a follow-up PR.

---

_Merged by @snowsignal on 2024-06-21 17:53_

---

_Closed by @snowsignal on 2024-06-21 17:53_

---

_Branch deleted on 2024-06-21 17:53_

---

_Comment by @MichaReiser on 2024-06-21 19:33_

This is helpful. Thanks for pointing it out. What's unclear to me is why closing a document in `didClose` that doesn't exist is just a "debug" message. That means, we expect valid cases where a document has already been removed when the handler is executed. To me it isn't clear when and why this is the case. A short inline comment explaining when it might happen that a document doesn't exist and why that's okay would I find helpful.

---
