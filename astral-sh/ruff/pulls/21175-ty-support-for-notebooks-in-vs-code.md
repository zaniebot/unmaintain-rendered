```yaml
number: 21175
title: "[ty] Support for notebooks in VS Code"
type: pull_request
state: merged
author: MichaReiser
labels:
  - notebook
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/lsp-notebooks-phase2
created_at: 2025-10-31T20:21:20Z
updated_at: 2025-11-13T12:26:06Z
url: https://github.com/astral-sh/ruff/pull/21175
synced_at: 2026-01-12T15:57:18Z
```

# [ty] Support for notebooks in VS Code

---

_@MichaReiser_

## Summary

This PR adds support for notebook synchronization to ty's LSP.
ty now supports LSP operations across LSP cell boundaries and no longer incorrectly
reports diagnostics when using variables that were defined in earlier cells. 

Most of the changes are integrating notebooks into ty's LSP range to text-range conversion. However, this PR also introduces a new `system.source_type` method to ensure the editor and ty agree on what's considered a notebook. 


Fixes https://github.com/astral-sh/ty/issues/173

## Known limitations

The LSP doesn't support cross-cell edits for completions. This is a problem for auto-imports because the importer will try to insert the import in the first cell, rather than the current cell. Given that auto import is still experimental, I suggest tackling this in a follow up PR.

Another limitation is that we can't show diagnostics for notebooks or references within notebooks unless they're open in the editor. This is because ty can't know the URI for a cell unless the notebook is open, which prevents us from mapping the diagnostic to a meaningful location. The best we could do is to map the location to the start of the notebook but this also seems bad (and notebooks don't support pull or workspace diagnostics...). For now, we match what pylance does and only handle open notebooks.



## Test Plan

Added E2E tests


https://github.com/user-attachments/assets/cbd9b873-a840-4e86-b280-d5d0d5a7d739



---

_Label `server` added by @AlexWaygood on 2025-10-31 20:23_

---

_Label `ty` added by @AlexWaygood on 2025-10-31 20:23_

---

_Comment by @github-actions[bot] on 2025-10-31 20:41_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-10-31 20:42_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-10-31 20:44_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/semantic_tokens.rs`:241 on 2025-10-31 20:44_

This fixes a bug where ty highlighted a token that started right at the end of the provided range. This resulted in a panic for notebooks because ty tried to highlight a token from the next cell, which isn't allowed 

---

_@MichaReiser reviewed on 2025-10-31 20:45_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/notebook.rs`:254 on 2025-10-31 20:45_

TODO, delete

---

_@MichaReiser reviewed on 2025-10-31 20:45_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/range.rs`:84 on 2025-10-31 20:45_

TODO: Handle more gracefully

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:99 on 2025-10-31 20:47_

We can't publish diagnostics for notebooks that aren't open because we don't know the URLs of the cells. But VS code uses `didChangeNotebook` for open notebooks and not `didChangeWatchedFiles`. That's why this comment isn't relevant.

---

_@MichaReiser reviewed on 2025-10-31 20:47_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/diagnostic.rs`:46 on 2025-10-31 20:48_

Delete

---

_@MichaReiser reviewed on 2025-10-31 20:48_

---

_Label `notebook` added by @MichaReiser on 2025-11-06 20:42_

---

_Comment by @github-actions[bot] on 2025-11-07 17:09_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @MichaReiser on 2025-11-07 17:39_

---

_Review requested from @carljm by @MichaReiser on 2025-11-07 17:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-07 17:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-11-07 17:39_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-07 17:39_

---

_Review request for @sharkdp removed by @sharkdp on 2025-11-11 08:17_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-11 11:14_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-11 11:14_

---

_Review requested from @Gankra by @MichaReiser on 2025-11-11 11:14_

---

_Review comment by @Gankra on `crates/ruff_db/src/source.rs`:48 on 2025-11-11 14:19_

wow not very forward thinking, micha, what if typeshed adds stubs for the most popular notebooks in the python ecosystem!!!! /silly

---

_Review comment by @Gankra on `crates/ruff_db/src/system.rs`:81 on 2025-11-11 14:22_

It's probably worth adding a comment to these two functions explaining that we're defaulting to "system has no additional info" (and so the caller will defer to e.g. the file extension)

---

_Review comment by @Gankra on `crates/ruff_notebook/src/notebook.rs`:401 on 2025-11-11 14:27_

Checking that this actually handles the last cell properly? We store a trailing offset?
(I assume this will be elucidated elsewhere...)

---

_Review comment by @Gankra on `crates/ty_server/src/document/notebook.rs`:13 on 2025-11-11 14:37_

I can't understand what this is saying

---

_Review comment by @Gankra on `crates/ty_server/src/document/notebook.rs`:142 on 2025-11-11 14:46_

Obscure collections API being useful!!! 🎊 

(Had to actually check that `1..1` works the way you want, it does)

---

_Review comment by @Gankra on `crates/ty_server/src/document/notebook.rs`:152 on 2025-11-11 14:50_

Slightly unfortunate that this logic is duplicated but it's so trivial it's whatever

---

_Review comment by @Gankra on `crates/ty_server/src/document/notebook.rs`:150 on 2025-11-11 14:53_

This code profoundly confused me (the key here is that `cells` isn't `self.cells` or `new_cells`). Either it should have a comment or some of these cells need more descriptive names.

---

_Review comment by @Gankra on `crates/ty_server/src/document/notebook.rs`:281 on 2025-11-11 14:58_

I wonder if I'll ever find the new home for these tests... (or are they meaningless?)

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:22 on 2025-11-11 15:02_

```suggestion
// The real challenge here is that `to_lsp_range` now needs to
```

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:25 on 2025-11-11 15:04_

...also this is a really weird comment outside of the context of reviewing this PR. Was this a note to yourself during hacking?

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/diagnostics.rs`:162 on 2025-11-11 15:06_

Might be good to add a comment explaining why notebooks need to bypass this

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_change_notebook.rs`:7 on 2025-11-11 15:10_

Slightly odd for notifications to yoink code from eachother like this

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_close.rs`:40 on 2025-11-11 15:12_

Compute is_cell earlier so you can use it in the check above

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_close.rs`:47 on 2025-11-11 15:14_

```suggestion
            // Notebooks only support push diagnostics.
```

(Genuinely forget, but I am very sad if the terminology is really "publish" and "pull" and not "push" and "pull")

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/notifications/did_open.rs`:48 on 2025-11-11 15:15_

`open_document`?

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:53 on 2025-11-11 15:19_

This can be one big && chain instead of nested ifs

---

_@Gankra approved on 2025-11-11 15:23_

Incredible work!

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:36 on 2025-11-12 04:29_

Can you update the documentation for `NotebookCell` now that it doesn't contain the `TextDocument` in it?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:39 on 2025-11-12 04:29_

I'm curious to know why is this being added?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:80 on 2025-11-12 04:31_

Would it be useful to add a debug log if the index doesn't contain the text document representing the cell?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/text_document.rs`:22 on 2025-11-12 05:01_

IIUC, this is removed because the server will always need to re-compute the index whenever the content in a text document changes so there's no benefit of computing this ahead of time?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:162 on 2025-11-12 05:14_

Agree, I think we should update the function documentation where "This function is a no-op..." is mentioned.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:144 on 2025-11-12 05:16_

Is there a specific reason this has been moved down after the document snapshotting? This seems like a small optimization to avoid taking the snapshot if the client uses pull diagnostics.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:116 on 2025-11-12 05:19_

Why is this (`*_if_needed` variant of `clear_diagnostics`) required when a single `publish_diagnostics` is sufficient?

It might be worth adding a brief documentation for what does "if needed" mean here.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:23 on 2025-11-12 05:21_

Is this a leftover comment? Or, which part of the code is this referring to?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:149 on 2025-11-12 05:27_

I think we could rename this to `publish_diagnostics_if_needed` to be consistent with the `clear_diagnostics_*` version.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:116 on 2025-11-12 05:43_

Oh, is the reason that a document representing the cell doesn't really exists on the ty side and it only exists in the LSP index which means that when closing a document representing a notebook cell, it needs to take a different logic (as is done using the `document.is_cell` check)?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_close.rs`:37 on 2025-11-12 05:45_

It might be worth adding a comment for posterity to explain why this is not being done for a document representing a notebook cell. Refer to my other comment in `diagnostics.rs` as I got confused why cell document needs to be handled uniquely but I understand it now.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:99 on 2025-11-12 05:52_

We have the logic for the Ruff language server to do this, is that not relevant here? The Ruff server will update the diagnostics for all the _open_ notebooks.

Edit: Ok, I just tested this out and VS Code does send the `didChangeNotebook` notification but it is also sending the `didChangeWatchedFiles` notification which I guess is irrelevant here.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/index.rs`:76 on 2025-11-12 05:59_

Is there a specific reason for using `unwrap_or_default` here instead of `if let ...` ?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:151 on 2025-11-12 06:07_

```suggestion
                    .map(|(index, cell)| (cell.url.clone(), index)),
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:144 on 2025-11-12 06:09_

```suggestion
        // Re-build the cell-index if new cells were added or deleted
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:124 on 2025-11-12 06:12_

💯 I really like how the `update` method is simplified given that it has been the source of multiple hard to find bugs in the Ruff server !!

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/range.rs`:151 on 2025-11-12 06:20_

This seems more like an `absolute_position` instead of the absolute start?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/range.rs`:193 on 2025-11-12 06:22_

Unrelated to this PR, but there seems to be a some typo in the documentation for `TextSizeExt::to_lsp_position` and `ToRangeExt::to_lsp_range`:

```rs
    /// Converts self into a position into an LSP text document (can be a cell or regular document).
```

Should it be "Converts self into a position for an LSP text ..."?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/range.rs`:303 on 2025-11-12 06:26_

I think it might be useful to expand this comment with a reason on why this is necessary (I'm not exactly sure why).

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:43 on 2025-11-12 06:32_

It might be worth renaming this to `cell_range` instead with a comment stating that this is required to limit the semantic token generation for the cell as `file` would represent an entire notebook.

---

_Review comment by @dhruvmanila on `crates/ty_server/tests/e2e/main.rs`:444 on 2025-11-12 06:41_

At first, I thought this was collecting `N` number of diagnostics but that's not true, maybe we can rename this to `send_publish_diagnostics_requests` with documentation as "Send `N` publish diagnostics requests and collect the diagnostics into ..."?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/notebook.rs`:281 on 2025-11-12 06:44_

I think this has been ported over to use the E2E test framework, is that correct @MichaReiser ?

---

_@dhruvmanila approved on 2025-11-12 06:45_

This is really great!

---

_@MichaReiser reviewed on 2025-11-12 07:48_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/notebook.rs`:281 on 2025-11-12 07:48_

Yes, I ported this to an E2E test (which I'm very glad that I did because I made the same mistake as we had in ruff's first notebook implementation 😆)

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/text_document.rs`:22 on 2025-11-12 07:52_

The main reason we stored the `index` in Ruff is because we use `document.index` to map positions from text ranges to LSP ranges (and vice versa). However, this isn't the case for ty. In ty, we use `line_index` which has its own caching. 

The only remaining place where `index` was used was in the `document.update`, which we only need to compute when a document changes and the index is outdated immediately after. I didn't bother trying to use `line_index` in `document.update`. We could, but it's probably not worth the complexity.

---

_@MichaReiser reviewed on 2025-11-12 07:52_

---

_@dhruvmanila reviewed on 2025-11-12 08:53_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/document/text_document.rs`:22 on 2025-11-12 08:53_

That makes sense, thanks for the explanation!

---

_@MichaReiser reviewed on 2025-11-12 16:37_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_change_watched_files.rs`:99 on 2025-11-12 16:37_

I'm surprised that it sends both. That seemms like bug in VS Code. For non-notebooks, VS Code sends a `didChange` event when the file is open and a `didChangeWatchedFiles` event otherwise. 

`didChangeWatchedFiles` isn't relevant for notebooks because we can't do anything useful with them when they're closed. That's why we skip them here.

---

_@MichaReiser reviewed on 2025-11-12 16:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:48 on 2025-11-12 16:38_

Lol. I'll update it when you change our module resolver to work with notebooks ;)

---

_@MichaReiser reviewed on 2025-11-12 16:44_

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/notebook.rs`:401 on 2025-11-12 16:44_

`CellOffset` contains:

* the offset `0` (even if the notebook has no cells)
* the end offset of every cell

Let's assume the notebook has one cell where the cell's length is 20 bytes. The build cell offsets then contains `[0, 20]`. Retrieving the range for cell 0 returns `0..20`. 

The function returns `None` if called with an out-of-bounds cell. E.g. `cell_range(1)` succeeds to look up the start offset (20), but it fails to retrieve the end offset and returns `None` instead

---

_@MichaReiser reviewed on 2025-11-12 16:47_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/notebook.rs`:142 on 2025-11-12 16:47_

Yes, I was very pleased when I found it. I first did some obscure reverse insertion in combination with `insert`. Not only did I get it wrong in the first place, it also would have had horrible performance. 

However, it took me a surprising long time to find it. Surprising because this is/was the main array manipulation API in JavaScript. So I should have tried this first.

---

_@MichaReiser reviewed on 2025-11-12 16:51_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:25 on 2025-11-12 16:51_

Oops, sorry. Yes, this is just me taking notes and forgetting to clean up.... Am I an AI?

---

_@MichaReiser reviewed on 2025-11-12 16:52_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_close.rs`:47 on 2025-11-12 16:52_

I'm sorry that I have to break this to you but...

`textDocument/publishDiagnostics`



---

_@MichaReiser reviewed on 2025-11-12 16:55_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/notifications/did_open.rs`:48 on 2025-11-12 16:55_

Hmm, maybe. I used `opened` to indicate that it should be called **after** the document was opened. Similar to the Reacts callbacks `onClicked` or `onChanged`. But let me think if there's a better name

---

_@MichaReiser reviewed on 2025-11-12 17:01_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/semantic_tokens.rs`:53 on 2025-11-12 17:01_

It will take me forever to get used to let-chains... But you can see, I tried.

---

_@MichaReiser reviewed on 2025-11-12 17:04_

---

_Review comment by @MichaReiser on `crates/ty_server/src/document/notebook.rs`:39 on 2025-11-12 17:04_

It's so that we can map it to `ruff_notebook::CodeCell::execution_count` (the naming here is a bit weird but I'm 99% sure it's the same)

---

_@MichaReiser reviewed on 2025-11-12 17:11_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:144 on 2025-11-12 17:11_

We can't use pull diagnostics if the document is a cell or notebook and we need to snapshot the document to make this decision.

---

_Comment by @codspeed-hq[bot] on 2025-11-12 17:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Flsp-notebooks-phase2?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21175 will **not alter performance**

<sub>Comparing <code>micha/lsp-notebooks-phase2</code> (163d419) with <code>main</code> (cd183c5)</sub>



### Summary

`✅ 52` untouched  





---

_@dhruvmanila reviewed on 2025-11-13 08:36_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:144 on 2025-11-13 08:36_

Right, but we can still make the decision about whether to even take the snapshot if the client doesn't support pull diagnostics at all.

---

_@MichaReiser reviewed on 2025-11-13 09:24_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:144 on 2025-11-13 09:24_

I'm not sure I follow. We only early exit if the client supports pull diagnostics **and** it's not a notebook cell and we need the document to make this decision. 

Also, taking a snapshot is fairly cheap (a few clones and bumping a few `Arc` counters)



---

_@MichaReiser reviewed on 2025-11-13 09:37_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/index.rs`:76 on 2025-11-13 09:37_

It avoids very deeply nested `if let` chains because there are so many fields that can be `None`. 

---

_@MichaReiser reviewed on 2025-11-13 09:45_

---

_Review comment by @MichaReiser on `crates/ty_server/tests/e2e/main.rs`:444 on 2025-11-13 09:45_

I can rename it to `collect_publish_diagnostic_notifications`. Renaming it to `send` seems incorrect to me as it isn't sending any messages.

---

_Merged by @MichaReiser on 2025-11-13 12:23_

---

_Closed by @MichaReiser on 2025-11-13 12:23_

---

_Branch deleted on 2025-11-13 12:23_

---
