```yaml
number: 11206
title: "`ruff server`: Support Jupyter Notebook (`*.ipynb`) files"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jupyter-notebook
created_at: 2024-04-29T17:49:46Z
updated_at: 2024-05-21T22:44:43Z
url: https://github.com/astral-sh/ruff/pull/11206
synced_at: 2026-01-10T22:05:26Z
```

# `ruff server`: Support Jupyter Notebook (`*.ipynb`) files

---

_Pull request opened by @snowsignal on 2024-04-29 17:49_

## Summary

Closes https://github.com/astral-sh/ruff/issues/10858.

`ruff server` now supports `*.ipynb` (aka Jupyter Notebook) files. Extensive internal changes have been made to facilitate this, which I've done some work to contextualize with documentation and an pre-review that highlights notable sections of the code.

`*.ipynb` cells should behave similarly to `*.py` documents, with one major exception. The format command `ruff.applyFormat` will only apply to the currently selected notebook cell - if you want to format an entire notebook document, use `Format Notebook` from the VS Code context menu.

## Test Plan

The VS Code extension does not yet have Jupyter Notebook support enabled, so you'll first need to enable it manually. To do this, checkout the `pre-release` branch and modify `src/common/server.ts` as follows:

Before:
![Screenshot 2024-05-13 at 10 59 06 PM](https://github.com/astral-sh/ruff/assets/19577865/c6a3c604-c405-4968-b8a2-5d670de89172)

After:
![Screenshot 2024-05-13 at 10 58 24 PM](https://github.com/astral-sh/ruff/assets/19577865/94ab2e3d-0609-448d-9c8c-cd07c69a513b)

I recommend testing this PR with large, complicated notebook files. I used notebook files from [this popular repository](https://github.com/jakevdp/PythonDataScienceHandbook/tree/master/notebooks) in my preliminary testing.

The main thing to test is ensuring that notebook cells behave the same as Python documents, besides the aforementioned issue with `ruff.applyFormat`. You should also test adding and deleting cells (in particular, deleting all the code cells and ensure that doesn't break anything), changing the kind of a cell (i.e. from markup -> code or vice versa), and creating a new notebook file from scratch. Finally, you should also test that source actions work as expected (and across the entire notebook). 

Note: `ruff.applyAutofix` and `ruff.applyOrganizeImports` are currently broken for notebook files, and I suspect it has something to do with https://github.com/astral-sh/ruff/issues/11248. Once this is fixed, I will update the test plan accordingly.

---

_Label `server` added by @snowsignal on 2024-04-29 17:49_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-29 17:49_

---

_Comment by @github-actions[bot] on 2024-04-29 18:07_

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

_Review comment by @snowsignal on `crates/ruff_server/src/edit/range.rs`:150 on 2024-05-14 05:19_

Is there a better way we could handle this conversion?

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:29 on 2024-05-14 05:26_

Some context for this:
* This is the replacement for `Workspaces`, which used to be the encapsulating struct that stored all documents.
* `Index` uses a flat structure to store documents and workspace settings, which makes it marginally more efficient to look-up documents by URL. The big advantage, however, is that we can map arbitrary notebook cells to document paths really easily, without having to search each workspace in turn.
* I've moved the logic for mutating documents to here - in other words, `DocumentController` is private to `index.rs`, and to update documents you need to call the associated update function on `Index`. The reason why I made this change is because we need to update the notebook cell index after cells get added or deleted, and I wanted to centralize that logic here.

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:104 on 2024-05-14 05:32_

I need to consolidate this logic into a function, since we use this pattern in several places.

---

_Review comment by @snowsignal on `crates/ruff_server/src/fix.rs`:89 on 2024-05-14 05:46_

This needs some reworking to be easier to read.

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:34 on 2024-05-14 05:50_

For context: we now generate the text edits in advance and store that in the associated data struct, instead of generating them later.

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:81 on 2024-05-14 05:54_

Notebook documents don't have a line index, so we need to build a new line index from the combined source. The side effect of this is that we also re-generate the line index for regular documents instead of using their existing one.

(I'd like to eventually remove the line index from `TextDocument`, since at the moment we wastefully re-generate it whenever it gets updated - and we don't even use it in that many places now).

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:222 on 2024-05-14 06:26_

I also need to refactor this logic: it's used multiple times and isn't easy to read.

---

_@snowsignal reviewed on 2024-05-14 06:37_

I've left some comments to add context for a few parts of the code and also to remind myself to refactor a few things before we merge 🙂 

---

_Marked ready for review by @snowsignal on 2024-05-14 06:55_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-05-14 06:55_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:94 on 2024-05-14 14:13_

I'd prefer to move this over in `ruff_server` and expose the `from_raw_notebook` method. This way the separation between the `ruff_server` crate and `ruff_notebook` crate still remains.

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:136 on 2024-05-14 14:13_

```suggestion
    fn from_raw_notebook(
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit.rs`:48 on 2024-05-14 14:20_

I'm assuming this works because VS Code chooses a different scheme (`notebook-cell` I think?) for Notebook cell documents. And, I thought of the same thing to do in `ruff-lsp` initially but it turns out that this is specific to VS Code and possibly wouldn't work across editors. The reason is that the spec mentions to not rely on the URI format:

> Cell text documents have an URI however servers should not rely on any format for this URI since it is up to the client on how it will create these URIs.
>
> Reference: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#notebookDocument_synchronization (Point 2)

This would work today because I think only VS Code supports Jupyter Notebook natively in the editor. But, it might not work for other editors (possibly Zed in the future).

The `ruff-lsp` implementation basically uses lookups to determine the document this URI belongs to. Basically, it'll check if there's a cell document with the URI and if it does, it's classified as a cell document. Similar lookup is done for Notebook document and at the end fall back to consider it as a text document.

Looking at the changes in this PR, I'm assuming something like that can be implemented by querying the `Index` although you'll have better context on how can this be implemented.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit.rs`:48 on 2024-05-14 14:23_

Looking at the references of `DocumentKey::from_url`, it does seem that the references have access to the `Index`.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit.rs`:59 on 2024-05-14 14:25_

Based on your comment, does this need to be a runtime assertion or is it fine to keep it in the debug build?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-14 14:28_

nit: we could utilize `ruff_index::newtype_index` for `cells` and `cell_index`. This way the indexing would be in a type-safe manner. For example,

https://github.com/astral-sh/ruff/blob/4ddd930c60cdda26b495bff0e4a22c72b1c682ab/crates/ruff_python_semantic/src/binding.rs#L330-L342

In this case, it would be something like:
```rs
#[newtype_index]
pub struct CellId;

pub struct NotebookCells(IndexVec<CellId, NotebookCell>);
```

This way we use `CellId` instead of a random `usize` to index into the `NotebookCell` vector.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:87 on 2024-05-14 14:29_

nit: the panic would indicate a logic error, right? If so, can we include the `NotebookError` here just for context?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:86 on 2024-05-14 14:31_

As mentioned in the related comment above, we can directly utilize `from_raw_notebook` API here.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:30 on 2024-05-14 14:39_

I think this might be ok but an alternate way to model this would be to only store the URI of the text document in `document` field. The actual `TextDocument` would be stored centrally where the other `TextDocument` (representing the Python file) is being stored. This would introduce an indirection if we want to get the cell content but it would keep the representation similar to the spec.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit.rs`:48 on 2024-05-14 14:41_

It's also mentioned a bit below in the same reference section:

> Syncing the text content of a cell is relatively easy since clients should model them as text documents. However since the URI of a notebook cell’s text document should be opaque, servers can not know its scheme nor its path. However what is know is the notebook document itself. We therefore introduce a special filter for notebook cell documents:

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:107 on 2024-05-14 14:46_

Are we sure that this unwrap is safe? Would it be better to use `unwrap_or_default`? I guess the latter could lead to an invalid internal state.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:117 on 2024-05-14 14:47_

nit: I wonder if we can utilize the information from the `structure` field to update the index instead of rebuilding it from scratch.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:26 on 2024-05-14 14:49_

I think it will be useful to add some documentation on the struct, fields and it's various methods. There are lots of methods and it's useful as a developer to know in what scenario would a certain method be useful 😅 

---

_@dhruvmanila reviewed on 2024-05-14 14:56_

I've provided some initial comments and I plan to look at other changes soon. It's quite a big PR :)

It would be helpful if you could briefly enumerate all the changes made in the PR description. I'd especially find the list of refactoring useful. It would be also useful for posterity to provide any context on the reasoning behind the implementation and if there were any other solutions that you thought of but didn't implement it.

Can you also update the test plan with all the cases that you've tested this against? Ideally it could contain the VS Code settings used and the action performed (command, code action, etc.). There are so many VS Code settings, commands and code actions for Notebook 😅

---

_@snowsignal reviewed on 2024-05-15 05:27_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit.rs`:59 on 2024-05-15 05:27_

This assertion is more of a sanity check for a developer than something we'd actually want to crash the extension over, which is why I've left it in the debug build.

---

_@snowsignal reviewed on 2024-05-15 08:14_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-15 08:14_

I'm not sure if this approach works in this specific case, because `IndexVec` doesn't support insertions (which we need for notebook cell array updates).

---

_@snowsignal reviewed on 2024-05-15 08:21_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:30 on 2024-05-15 08:21_

This would add complexity to document queries, since we can't just fetch documents from the index dynamically in a background thread, so we'd have to fetch all cell text documents in advance and store them in a separate index in the query. I'm OK with our internal representation not matching the LSP specification if it makes things simpler.

---

_@snowsignal reviewed on 2024-05-15 08:30_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:107 on 2024-05-15 08:30_

This is converting from `u32` to `usize`, so it should be safe as long as you aren't running this on a 16-bit architecture (and even then, you'd need to have a notebook with 65k+ indexes or more).

---

_@snowsignal reviewed on 2024-05-15 08:33_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit.rs`:48 on 2024-05-15 08:33_

You're right, we could utilize the `Index` here instead of looking for things that may vary by implementation 👍 

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-15 12:39_

By insertions, do you mean inserting a new cell in the vector? We can provide methods on `NotebookCells`:

```rs
pub struct NotebookCells(IndexVec<CellId, NotebookCell>);

impl NotebookCells {
	pub fn push(&mut self, cell: NotebookCell) -> CellId {
		self.0.push(cell)
	}
}
```

Refer
https://github.com/astral-sh/ruff/blob/2952263db62372df547dd9106d91ce8650697d25/crates/ruff_index/src/vec.rs#L61-L66

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:107 on 2024-05-15 12:43_

Oh, then I think you can directly do `structure.array.start as usze`?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/range.rs`:21 on 2024-05-15 12:53_

nit: I would possibly introduce a new type or a struct here to indicate that the `usize` is the cell index and the range is for the corresponding cell:

```rs
pub struct NotebookRange {
	cell: usize,
	range: types::Range,
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/range.rs`:150 on 2024-05-15 13:01_

Yeah, I remember this. It's because the cell content are concatenated by a newline but the newline doesn't actually exists in the original source code. So, when an edit tries to remove the entire line, the range includes the newline. This means the edit range goes out of the cell. I thought we fixed this. Let me check the code.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:81 on 2024-05-15 13:35_

Can we separate the logic for Notebook and text document to avoid building the index multiple times? But, I see that you want to get remove the index altogether for text document so maybe this isn't worth looking into right now.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:154 on 2024-05-15 13:37_

nit: would it be possible to combine this in a single `if` condition? It would be easier to reason about when everything related to notebook is in the same block and has access to the notebook.

```rs
if let Some(notebook) = query.as_notebook() {
	// .. do everything with the notebook
} else {
	// .. do everything with the text document
}
```

You can possible store the LSP diagnostics in a variable:
```rs
    let mut lsp_diagnostics = data
        .into_iter()
        .zip(noqa_edits)
        .map(|(diagnostic, noqa_edit)| {
            to_lsp_diagnostic(diagnostic, &noqa_edit, &source_kind, &index, encoding)
        });
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:260 on 2024-05-15 13:40_

I think this should be `true`. The client should notify us when a Notebook is saved. This is true for `ruff-lsp`:

https://github.com/astral-sh/ruff-lsp/blob/b8a48009f35756dc51268beb6bbc39c21a1e3a46/ruff_lsp/server.py#L139-L139

Which then requires the server to implement the `notebookDocument/didSave` endpoint:

https://github.com/astral-sh/ruff-lsp/blob/b8a48009f35756dc51268beb6bbc39c21a1e3a46/ruff_lsp/server.py#L533-L538

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:266 on 2024-05-15 13:44_

Is this specific to VS Code? I'm referring to the "jupyter-notebook" type. For reference, we don't use any notebook document filter in `ruff-lsp`:

https://github.com/astral-sh/ruff-lsp/blob/b8a48009f35756dc51268beb6bbc39c21a1e3a46/ruff_lsp/server.py#L129-L140

This asks to only send the request to sync the Python code cells as we aren't interested in any kind of cell.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api.rs`:95 on 2024-05-15 13:45_

I might be missing some context here but can you tell me why is the `DidSaveNotebookDocument` not implemented?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_close.rs`:39 on 2024-05-15 13:50_

Huh, I wasn't aware of this but it actually does send the requests to close each cell first and then the notebook:
```
2024-05-15 19:19:48.235 [info] [Trace - 7:19:48 PM] Sending notification 'textDocument/didClose'.
2024-05-15 19:19:48.236 [info] [Trace - 7:19:48 PM] Sending notification 'textDocument/didClose'.
2024-05-15 19:19:48.236 [info] [Trace - 7:19:48 PM] Sending notification 'textDocument/didClose'.
2024-05-15 19:19:48.236 [info] [Trace - 7:19:48 PM] Sending notification 'textDocument/didClose'.
2024-05-15 19:19:48.236 [info] [Trace - 7:19:48 PM] Sending notification 'textDocument/didClose'.
2024-05-15 19:19:48.236 [info] [Trace - 7:19:48 PM] Sending notification 'notebookDocument/didClose'.
```

I guess it's not a problem in `ruff-lsp` because of the way the Notebook and cell documents are modeled in `pygls`.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_open_notebook.rs`:40 on 2024-05-15 13:53_

I'm not sure if we should use this guard. What's your motivation for this? I don't think we have anything like this in `ruff-lsp`. Did you try this with any other editor that supports Jupyter Notebook and can run ruff?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session.rs`:78 on 2024-05-15 13:56_

What happens if it doesn't? Is this function subject to a panic? If so, I'd add it in the function docs.

---

_@dhruvmanila reviewed on 2024-05-15 13:58_

---

_@dhruvmanila reviewed on 2024-05-15 14:01_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:66 on 2024-05-15 14:01_

By the comment, do you mean that the `Notebook` variant either represents a specific cell or an entire Notebook? I'd expand your comment, especially what does "entire notebook file is selected" mean.

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-15 18:44_

By 'insertions', I'm specifically referring to insertions at an index, like the `Vec::insert` method (which shifts up indexes after it).

---

_@snowsignal reviewed on 2024-05-15 18:44_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:107 on 2024-05-16 00:45_

Okay! I'm used to using the `try_from + unwrap` convention over using `as`, but the latter does seem to be the convention for the rest of the codebase.

---

_@snowsignal reviewed on 2024-05-16 00:45_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:260 on 2024-05-16 01:01_

Is there a reason we should listen for `didSave` in addition to `didChange`?

---

_@snowsignal reviewed on 2024-05-16 01:01_

---

_@snowsignal reviewed on 2024-05-16 01:03_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api.rs`:95 on 2024-05-16 01:03_

At the moment, I don't see a need for us to handle `didSave` notifications in general (we don't handle `didSaveTextDocument` either).

---

_@snowsignal reviewed on 2024-05-16 01:08_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_open_notebook.rs`:40 on 2024-05-16 01:08_

> Did you try this with any other editor that supports Jupyter Notebook and can run ruff?

As far as I know the only other major editor that supports Jupyter Notebook is PyCharm, and we haven't got that working with `ruff server` yet.

This is meant to be a quick sanity check to ensure that we're only working with Python notebooks, but it's not super necessary.

---

_@snowsignal reviewed on 2024-05-16 01:08_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session.rs`:78 on 2024-05-16 01:08_

It throws an error - I'll add that to the docs.

---

_@snowsignal reviewed on 2024-05-16 01:09_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/index.rs`:66 on 2024-05-16 01:09_

> By the comment, do you mean that the Notebook variant either represents a specific cell or an entire Notebook?

That's correct - I'll try to clarify this.

---

_@dhruvmanila reviewed on 2024-05-16 03:48_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-16 03:48_

I see, thanks for clarifying. I leave this up to you but I think we should at least use a newtype wrapper (`pub type CellId = usize`) instead of raw `usize` as I've seen it used in a lot of places.

---

_@snowsignal reviewed on 2024-05-16 04:04_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/notebook.rs`:22 on 2024-05-16 04:04_

Sure, we can do that!

---

_@snowsignal reviewed on 2024-05-16 04:07_

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:81 on 2024-05-16 04:07_

We could separate the logic but I don't think it's worth the technical investment IMO if we plan to eventually remove this anyway.

---

_@snowsignal reviewed on 2024-05-16 04:38_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:266 on 2024-05-16 04:38_

I got this from the LSP specification:
<img width="664" alt="Screenshot 2024-05-15 at 9 37 34 PM" src="https://github.com/astral-sh/ruff/assets/19577865/62f64858-46ed-4664-a8a3-b4be1a936012">

I assume this would be the same across editors.

---

_Review requested from @dhruvmanila by @snowsignal on 2024-05-16 04:39_

---

_@dhruvmanila reviewed on 2024-05-16 04:57_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:260 on 2024-05-16 04:57_

For visibility, we discussed this on Discord and in conclusion it's fine to not implement the `didSave` handler because we _always_ refresh the diagnostics on `didChange` event. And, the new server doesn't support the `lint.run` setting from `ruff-lsp`.

---

_@dhruvmanila reviewed on 2024-05-16 05:07_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:266 on 2024-05-16 05:07_

I see. I'm not sure where does the `jupyter-notebook` type comes from.

> I assume this would be the same across editors.

I think it depends - If it is set by VS Code, then I don't think it would be same.

I don't think we need to worry about this now but, just to be on the safer side, I'd probably keep this same as `ruff-lsp`.

Does this work on unsaved Notebook files? Like, a new notebook file is opened in VS Code but it doesn't exists on disk yet.

---

_@dhruvmanila reviewed on 2024-05-16 05:09_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/notifications/did_open_notebook.rs`:40 on 2024-05-16 05:09_

I'm fine leaving it for now but it seems like a duplicate given the notebook filter the server has already applied.

---

_@dhruvmanila reviewed on 2024-05-16 05:24_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:12 on 2024-05-16 05:24_

I believe this comment is not needed now? Also, you'd need to use the `source_type` here to determine the parser mode because otherwise it errors for a notebook containing escape commands.

---

_@snowsignal reviewed on 2024-05-16 05:29_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:266 on 2024-05-16 05:29_

> Does this work on unsaved Notebook files? Like, a new notebook file is opened in VS Code but it doesn't exists on disk yet.

This does fail - I think it might be because an unsaved URL isn't a valid file path.

---

_@snowsignal reviewed on 2024-05-16 05:33_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/notifications/did_open_notebook.rs`:40 on 2024-05-16 05:33_

I'm fine with removing it.

---

_@snowsignal reviewed on 2024-05-16 05:36_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:266 on 2024-05-16 05:36_

Hmmm.... it seems like, in general, that the server doesn't support files created from `Ctrl+N` in VS Code if they aren't saved to the file system. I'll create a follow-up issue for this.

---

_Comment by @dhruvmanila on 2024-05-16 05:44_

I think there's another problem here. To demonstrate that refer:

### `ruff-lsp`

https://github.com/astral-sh/ruff/assets/67177269/ea494142-a2dc-4d59-b0db-98a28589f480

### `ruff server`

https://github.com/astral-sh/ruff/assets/67177269/0c6c2473-6bab-47ce-80d8-00d3b2588556

You can see that there's an additional newline which gets added at the end for `ruff server`. The reason this is happening is because the cell content are concatenated by a newline but the newline doesn't actually exists in the original source code.

The reason it doesn't happen in `ruff-lsp` is because we post-process the Notebook where we strip this additional newline before updating the cell content:

https://github.com/astral-sh/ruff/blob/75f302298c2b6d481a466c463389ce770aa38f6b/crates/ruff_notebook/src/notebook.rs#L243-L273

This post-processing doesn't happen for the new server because it uses a wrapper to lint / format (`lint.rs` and `format.rs`).

---

_@dhruvmanila reviewed on 2024-05-16 05:49_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:266 on 2024-05-16 05:49_

> This does fail, but only because new notebook cell url gets passed into `didOpenTextDocument`, which isn't expected. Do we need to support 'unattached' notebook cells?

I just checked, we do support it. It's probably because we never touch the filesystem in `ruff-lsp` and always work with stdin with `ruff` command.

---

_@dhruvmanila reviewed on 2024-05-16 12:20_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/range.rs`:150 on 2024-05-16 12:20_

@snowsignal Here is where I fix it:

https://github.com/astral-sh/ruff/blob/75f302298c2b6d481a466c463389ce770aa38f6b/crates/ruff_linter/src/message/json.rs#L110-L152

Related PR for additional context.

---

_Comment by @dhruvmanila on 2024-05-16 12:23_

## Notes from testing

1. Did you implement the `notebook.*` variants of the source code actions? It doesn't seem to be working. They are `notebook.source.fixAll` and `notebook.source.organizeImports` which are specific to Notebook. Here, the server will receive only the first cell and we're responsible on returning the edits for the entire notebook. I'm using the following VS Code settings:
	```json
	  "notebook.codeActionsOnSave": {
	    "notebook.source.fixAll": "explicit"
	  }
	```
	For reference, https://github.com/astral-sh/ruff-lsp/blob/b8a48009f35756dc51268beb6bbc39c21a1e3a46/ruff_lsp/server.py#L219-L229

2. This might be unrelated to this PR but I'm getting the following error quite frequently:

	```
	2024-05-16 17:34:08.101 [info]  196.046579s WARN ruff_server::server::api Received notification $/setTrace which does not have a handler.
	
	2024-05-16 17:34:10.471 [info]  198.416243s ERROR ruff_server::server::api An error occurred while running textDocument/didClose: Unable to take snapshot for document with URL file:///Users/dhruv/.pyenv/versions/3.7.17/lib/python3.7/random.py
	```

	It's usually occurs after the `setTrace` notification. The confusing part is that I have no idea why is it occurring on `textDocument/didClose` when I haven't closed any document.

3. Based on this in the PR description:
	> Note: `ruff.applyAutofix` and `ruff.applyOrganizeImports` are currently broken for notebook files, and I suspect it has something to do with #11248. Once this is fixed, I will update the test plan accordingly.

	Were you able to look into this and fix it? I'm unable to run any "Ruff:" prefixed commands except for "Format Document".



---

_@snowsignal reviewed on 2024-05-16 15:20_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit/range.rs`:150 on 2024-05-16 15:20_

Thanks! So it looks like this is just something we'll have to deal with.

---

_Comment by @snowsignal on 2024-05-16 15:38_

@dhruvmanila 

> Did you implement the notebook.* variants of the source code actions?

No, this still needs to be added.

> This might be unrelated to this PR but I'm getting the following error quite frequently

This is an unrelated bug that I've seen several times before. I'll open a proper issue for it.

> Were you able to look into this and fix it? I'm unable to run any "Ruff:" prefixed commands except for "Format Document".

No, this still needs to be fixed.

---

_Review requested from @dhruvmanila by @snowsignal on 2024-05-21 11:10_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/range.rs`:139 on 2024-05-21 14:49_

Can you provide a comment which documents these two cases and why they're necessary? It might be useful to provide a small example as well.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:51 on 2024-05-21 14:55_

Does this handle the `\r\n` case?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit.rs`:143 on 2024-05-21 14:56_

I think we do have it enabled in `ruff-lsp`. What kind of problem are we facing here? If it's not critical, it might be useful to open an issue as well.

---

_@dhruvmanila reviewed on 2024-05-21 14:59_

---

_@dhruvmanila approved on 2024-05-21 15:26_

This is great work! Thank you for being patient and addressing all the feedbacks. LGTM!

---

_Comment by @dhruvmanila on 2024-05-21 15:27_

You might want to rebase to make the CI green :)

---

_@snowsignal reviewed on 2024-05-21 15:36_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/format.rs`:51 on 2024-05-21 15:36_

~~It should, since we strip the `\n` first and then the `\r`.~~ My bad, this actually needs some correction.

---

_@snowsignal reviewed on 2024-05-21 17:48_

---

_Review comment by @snowsignal on `crates/ruff_server/src/edit.rs`:143 on 2024-05-21 17:48_

The issue here is that we pass in the notebook document version and use it as the cell version, which breaks commands like `applyAutofix` because that's not the version we're supposed to be using. The notebook API isn't set up yet to track the versions of individual cell documents and I'd rather handle this in a follow-up.

---

_Merged by @snowsignal on 2024-05-21 22:29_

---

_Closed by @snowsignal on 2024-05-21 22:29_

---

_Branch deleted on 2024-05-21 22:29_

---
