```yaml
number: 11588
title: "`ruff server`: Add support for documents not exist on disk"
type: pull_request
state: merged
author: T-256
labels:
  - server
assignees: []
merged: true
base: main
head: textDocument/didOpen
created_at: 2024-05-28T21:43:56Z
updated_at: 2024-05-31T06:34:10Z
url: https://github.com/astral-sh/ruff/pull/11588
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server`: Add support for documents not exist on disk

---

_@T-256_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
before this PR `ruff server` at first layer checked that url received from lsp-client most matched `file://` and existed on disk, there are some scenarios that urls are not always `file://`-scheme. After this PR ruff server would also support urls other than `file://`
For example:

>url: `untitled:Untitled-1.ipynb?jupyter-notebook`
>
>url.scheme: `untitled`
>url.path: `Untitled-1.ipynb`
>url.quey: `jupyter-notebook`

Closes https://github.com/astral-sh/ruff/issues/11510
## Test Plan

<!-- How was it tested? -->
Open VSCode
Enable ruff extension's native server option
`Ctrl+Win+Alt+N` -> select `Python File`
Try write some codes and verify ruff works

---

_Comment by @github-actions[bot] on 2024-05-28 22:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/notifications/did_change_workspace.rs`:22 on 2024-05-29 07:22_

Nit: You can remove the `clone` here by removing the `ref` from `uri`
```suggestion
        for types::WorkspaceFolder { uri, .. } in params.event.added {
            session.open_workspace_folder(uri);
```

---

_@MichaReiser reviewed on 2024-05-29 07:22_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:53 on 2024-05-29 07:23_

Nit: Can we rename the field to `file_url` (same for notebook) now that it is no longer a path?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:92 on 2024-05-29 07:24_


```suggestion
            .map(|(url, _)| url.clone())
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:88 on 2024-05-29 07:25_

I think we can now change the signature to return a reference to the URL to avoid the cloning
```suggestion
    pub(super) fn text_document_urls(&self) -> impl Iterator<Item = &Url> + '_ {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:100 on 2024-05-29 07:25_


```suggestion
    pub(super) fn notebook_document_urls(&self) -> impl Iterator<Item = &Url> + '_ {
        self.documents
            .iter()
            .filter(|(_, doc)| doc.as_notebook().is_some())
            .map(|(url, _)| url)
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:134 on 2024-05-29 07:27_

Clippy is right here. The function never returns `None` and, therefore, the return type can simply be `DocumentKey`. We should change the signature instead of suppressing the lint.
```suggestion
    pub(super) fn key_from_url(&self, url: &Url) -> DocumentKey {
        if self.notebook_cells.contains_key(url) {
            DocumentKey::NotebookCell(url.clone())
        } else if url.path().ends_with("ipynb") {
            DocumentKey::Notebook(url.clone())
        } else {
            DocumentKey::Text(url.clone())
        }
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:134 on 2024-05-29 07:33_

I think we should also change the signature to take an owned `url` because we always clone the URL unconditionally (same for `Session::take_snapshot`). This will allow us to avoid some unnecessary clones over all.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:177 on 2024-05-29 07:34_


```suggestion
    pub(super) fn open_workspace_folder(&mut self, url: Url, global_settings: &ClientSettings) {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:184 on 2024-05-29 07:34_


```suggestion
        workspace_url: Url,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:129 on 2024-05-29 07:37_

This should probably be `.ipynb`
```suggestion
        if url.path().ends_with(".ipynb") {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:207 on 2024-05-29 07:39_


```suggestion
    pub(super) fn close_workspace_folder(&mut self, workspace_url: &Url) -> crate::Result<()> {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:220 on 2024-05-29 07:40_

Nit: To avoid calling `workspace_url.path()` for every single document and cell.
```suggestion
        let workspace_path = workspace_url.path();
        self.settings.remove(workspace_path).ok_or_else(|| {
            anyhow!(
                "Tried to remove non-existent folder {}",
                workspace_path
            )
        })?;
        // O(n) complexity, which isn't ideal... but this is an uncommon operation.
        self.documents
            .retain(|path, _| !path.path().starts_with(workspace_path));
        self.notebook_cells
            .retain(|_, path| !path.path().starts_with(workspace_path));
        Ok(())
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:333 on 2024-05-29 07:40_


```suggestion
    fn url_for_key<'a>(&'a self, key: &'a DocumentKey) -> Option<&'a Url> {
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:360 on 2024-05-29 07:41_


```suggestion
        file_url: Url,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:470 on 2024-05-29 07:45_

I think this should return the `url` unchanged and not just the `path` segment of it. 
```suggestion
    pub(crate) fn url(&self) -> &Url {
        match self {
            Self::Text { file_path, .. } | Self::Notebook { file_path, .. } => url,
        }
    }

```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:328 on 2024-05-29 07:46_

I think we should show the entire `url` instead of just the path
```suggestion
            anyhow::bail!("Document controller not available at `{}`", url);
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:290 on 2024-05-29 07:46_

I think we should show the entire document `url` instead of just the path segment
```suggestion
                url
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:259 on 2024-05-29 07:49_

The old implementation used to use `url.to_file_path` instead of `path`. Whats the reason for using `path` here?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:228 on 2024-05-29 07:50_

I think we should call `url.to_file_path` here instead of just taking the `path` part.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/fix.rs`:26 on 2024-05-29 08:03_

I think we shoudl use `to_file_path` here and we can fall back to `Untitled` if `to_file_path` fails because it is an untitled file (assuming that `to_file_path` fails in that case)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:68 on 2024-05-29 08:03_

Same as for `fix`. I think we should use `to_file_path` here.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:85 on 2024-05-29 08:04_


```suggestion
        let mut workspace_for_path = |url: lsp_types::Url| {
            let Some(workspace_settings) = workspace_settings.as_mut() else {
                return (path, ClientSettings::default());
            };
            let settings = workspace_settings.remove(&url).unwrap_or_else(|| {
                tracing::warn!("No workspace settings found for {url}");
                ClientSettings::default()
            });
            (path, settings)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:92 on 2024-05-29 08:04_


```suggestion
                workspace_for_url(folder.uri)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session.rs`:118 on 2024-05-29 08:04_

In the entire file. Rename `path` to `url` or `uri`

---

_@MichaReiser reviewed on 2024-05-29 08:11_

Thanks for working on this. Overall this looks good to me and is exactly what I wanted to do as well. 

I think the only part that I planned to add and isn't part of this PR is that we can't use `SourceType::from_path` for a new file and instead should respect the language setting coming from VS Code. In theory, this also applies for existing files because a user could have mapped all `.py` files to `notebooks` in their configuration. 

That's why I think we should infer the `SourceType` in the `didOpen` request from the [`item.languageId`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocumentItem). Unfortunately, the language id doesn't distinguish between python and python stub files. That's why `query.source_type()` still needs to look at the path if the source type communicated by VS code is Python. 

---

_Comment by @charliermarsh on 2024-05-29 19:42_

@T-256 - Do you want to address Micha's feedback, or want me to do so?

---

_Comment by @T-256 on 2024-05-29 21:12_

> Do you want to address Micha's feedback, or want me to do so?

I opened this PR as POC that what could be happened. I wanted to get time to test it yesterday, but unfortunately it didn't happen.
Good news! I tested now and worked as expected with new `nativeServer` enablement on VSCode and opening untitled file.
I'll update PR description accordingly.

@MichaReiser Thanks for review, few comments still unsolved, I'd be happy to Charlie help me solve them.


---

_Marked ready for review by @T-256 on 2024-05-29 21:12_

---

_Comment by @MichaReiser on 2024-05-30 09:24_

Thank you, @T-256 for all the work. I'll take a look at addressing the remaining review comments.

---

_Label `server` added by @MichaReiser on 2024-05-30 14:30_

---

_@MichaReiser reviewed on 2024-05-30 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:399 on 2024-05-30 14:39_

I'm not entirely sure about this. It's nice because the file is most likely to be part of the workspace and it would be unfortunate if we show lint errors for rules that are disabled in this workspace. 

However, it has the downside that the shown lint errors change when the user has an unsaved document and then opens a new workspace. A next pull diagnostic now uses the default configuration which is different from the last run. I think that's fine, considering that adding a workspace AND having an unsaved file is rare.

---

_@MichaReiser reviewed on 2024-05-30 14:39_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:550 on 2024-05-30 14:39_

I think it's worth to distinguish between the two. E.g., I think we shouldn't test for exclusion for unsaved files because we don't have a real path. 

---

_@T-256 reviewed on 2024-05-30 14:57_

LGTM, Thanks for done this.
Just for last question, shouldn't we explicitly specify what URL schemes we support? AFAIK and according to docs of [LSP specification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#documentFilter) it should be `file` or `untitled`. but URL itself could contain other schemes, IMO we should disallow other schemes. what do you think?

---

_@MichaReiser reviewed on 2024-05-30 15:09_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:262 on 2024-05-30 15:09_

I think users generally want this (take the settings from the root configuration in the only active workspace). But it does have the downside that adding a second workspace later on means that we suddenly take the fallback which changes the output. But I think that's fine.

---

_@T-256 reviewed on 2024-05-30 15:16_

---

_Review comment by @T-256 on `crates/ruff_server/src/session/index.rs`:262 on 2024-05-30 15:16_

I think there are such cases in vscode internals too. IIRC they solved by choosing first workspace was added. can we consider same at here?

---

_@MichaReiser reviewed on 2024-05-30 15:59_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:262 on 2024-05-30 15:59_

Possibly. We would need to keep a second vector that stores the workspace URLs in insertion order. I would prefer to leave this out of this PR for now.

---

_Comment by @MichaReiser on 2024-05-30 16:06_

I tested:

* Creating an untitled file
	*  In a single folder workspace: It used the configuration at the root of the folder if present, and otherwise used the fallback configuration
	* In a multi-folder workspace: It used the fallback configuration
	* The file doesn't get excluded
* I tested that, for an existing file
	* It uses the closest configuration
	* It doesn't get formatted when it is excluded
* It formats and lints an untitled notebook document 


I didn't change the code to use the language from the `didOpen` notification. I think we can wait with using that until we support more languages than Python

---

_@MichaReiser reviewed on 2024-05-30 16:07_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/fix.rs`:35 on 2024-05-30 16:07_

The way I implemented it here is that an untitled file can never be excluded. However, that means that an `exclude = ["*"]` won't apply where users might expect that it should. 

But other than that, I think not handling it in exclusion makes sense because we don't know in which folder the document will be stored.

---

_Review requested from @charliermarsh by @MichaReiser on 2024-05-30 16:08_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-05-30 16:08_

---

_Review request for @dhruvmanila removed by @MichaReiser on 2024-05-30 16:08_

---

_@charliermarsh reviewed on 2024-05-30 17:32_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index.rs`:550 on 2024-05-30 17:32_

I am wondering if we should make `Path` optional for the Ruff APIs like `lint_fix`...?

---

_@charliermarsh approved on 2024-05-30 17:32_

---

_@charliermarsh reviewed on 2024-05-30 17:32_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/fix.rs`:35 on 2024-05-30 17:32_

Makes sense.

---

_@charliermarsh reviewed on 2024-05-30 17:33_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/index.rs`:550 on 2024-05-30 17:33_

Like, could we remove this entirely? Why is the path needed in such cases? (If it's just for the extension, we could implement something special in `source_type`.)

---

_@MichaReiser reviewed on 2024-05-30 18:48_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:550 on 2024-05-30 18:48_

It's for diagnostics and maybe file based lint rules?

We might get away with it but I'm not very eager to change it. Like it would probably be an improvement but I rather finish res knot where this will be gone anyway 😅

---

_Merged by @MichaReiser on 2024-05-31 06:34_

---

_Closed by @MichaReiser on 2024-05-31 06:34_

---
