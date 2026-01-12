```yaml
number: 10597
title: "`ruff server` now supports `source.fixAll` source action"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/code-actions/source-level
created_at: 2024-03-26T05:15:45Z
updated_at: 2024-04-03T16:26:34Z
url: https://github.com/astral-sh/ruff/pull/10597
synced_at: 2026-01-12T15:55:32Z
```

# `ruff server` now supports `source.fixAll` source action

---

_@snowsignal_

## Summary

`ruff server` now has source action `source.fixAll` as an available code action.

This also fixes https://github.com/astral-sh/ruff/issues/10593 in the process of revising the code for quick fix code actions.

## Test Plan



https://github.com/astral-sh/ruff/assets/19577865/f4c07425-e68a-445f-a4ed-949c9197a6be



---

_Label `server` added by @snowsignal on 2024-03-26 05:15_

---

_Comment by @github-actions[bot] on 2024-03-26 05:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-03-26 07:48_

What do you think of implementing each checkbox in its own PR instead of having one large PR that implements all the features at once? To me, the individual features seem useful on its own and smaller PRs are easier to review, and tend to have a shorter lead time. 

---

_Comment by @charliermarsh on 2024-03-26 13:21_

👍 Smaller PRs means we can merge faster and even release these out in the wild faster too.

---

_Renamed from "`ruff server` now supports source actions `fixAll` and `organizeImports`, along with commands `applyAutofix` and `applyOrganizeImports`" to "`ruff server` now supports `source.fixAll` source action" by @snowsignal on 2024-03-26 14:27_

---

_Marked ready for review by @snowsignal on 2024-03-26 14:29_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-03-26 16:03_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:95 on 2024-03-26 17:01_

Nit
```suggestion
                    code: rule.noqa_code().to_string(),
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:199 on 2024-03-26 17:03_

Ruff's current LSP also supports a ruff specific `fixAll` action. It's [documented here](https://github.com/astral-sh/ruff-vscode?tab=readme-ov-file#configuring-vs-code) towards the end.

```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": "explicit",
      "source.organizeImports.ruff": "explicit"
    }
  }
}
```



---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:207 on 2024-03-26 17:04_

Nit: Should we move this into a `all()` method on `SupportCodeAction`. It could be easier to spot (and reuse) in case new variants are added.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:202 on 2024-03-26 17:07_

Nit: We could consider using a bitflag for this, considering that the action kinds are a fixed set. But I'm also okay keeping with we have, avoiding the allocation here is probably overkill.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:219 on 2024-03-26 17:27_

Nice trick to have a single iterator type. 

Nit: I do struggle reading this code. A lot is going on, including flattening. I wonder if the complexity is worth it to avoid a single `Vec` allocation, considering that we end up allocating a vec on line 174 anyway (we could avoid that by using an array instead).

I do find the non iterator version easier to reason about (I believe that's the equivalent code but I find it hard to say because I don't fully understand the iterator version)

````
    let edits_made: Vec<_> = edits
        .filter(|edit| {
            edit.diagnostic_fix
                .fix
                .applies(ruff_diagnostics::Applicability::Safe)
        })
        .collect();

    if edits_made.is_empty() {
        return Vec::new();
    }

    let diagnostics_fixed = edits_made
        .iter()
        .map(|edit| edit.original_diagnostic.clone())
        .collect();

    vec![
        types::CodeActionOrCommand::CodeAction(types::CodeAction {
            title: format!("{DIAGNOSTIC_NAME}: Fix all auto-fixable problems"),
            diagnostics: Some(diagnostics_fixed),
            kind: Some(types::CodeActionKind::SOURCE_FIX_ALL),
            edit: Some(types::WorkspaceEdit {
                document_changes: Some(types::DocumentChanges::Edits(
                    edits_made
                        .into_iter()
                        .flat_map(|edit| edit.document_edits.iter())
                        .cloned()
                        .collect(),
                )),
                ..Default::default()
            }),
            ..Default::default()
        }), // TODO: implement command handler for the server
            /*
            types::CodeActionOrCommand::Command(types::Command {
                ...
            }
             */
    ]
```

Edit: I think for now it would even be sufficient to return a single `Option`.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:82 on 2024-03-26 17:34_

Nit: I think I would just go with a `vec` here to keep things simple. We don't have any usages today where we want to pass an iterator
```suggestion
    diagnostics: Vec<types::Diagnostic>,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:129 on 2024-03-26 17:40_

Nit: You can avoid cloning here by using `deref`
```suggestion
        let title = edit
            .diagnostic_fix
            .kind
            .suggestion
            .as_deref()
            .unwrap_or(&edit.diagnostic_fix.kind.name);
```


---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:83 on 2024-03-26 17:46_

Nit: Considering that `available_actions` is a set (or bitset, see below), I would probably rewrite it as

```
			  // Add a quick fix for the diagnostic
        if available_actions.contains(&SupportedCodeAction::QuickFix) {
            response.extend(quick_fix(edits.iter()));
        }

				// Add a single fix all action for all diagnostics
        if available_actions.contains(&SupportedCodeAction::FixAll) {
            response.extend(fix_all(edits.iter()));
        }

        if available_actions.contains(&SupportedCodeAction::OrganizeImports) {
            todo!("Implement the `source.organizeImports` code action")
        }
```

I find this more readable because it makes the ordering explicit in code (no need to implement `Ord`) and allows for inline comments.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:52 on 2024-03-26 17:50_

I'm struggling a bit with the terminology: 

* A diagnostic has a list of edits
* The diagnostic gets converted into a `DiagnosticEdit` but it itself has a list of edits
* `diagnostic_edits` returns a vec of `DiagnosticEdit` (which is a list of edits). 

My recommendation would have been to rename `DiagnosticEdit` to `DiagnosticFix` but that type already exists and that type also stores a list of edits (but different edits?). 

I don't have a good recommendation on how to simplify this. Maybe rename `DiagnosticFix` to `SerializedFix` or `SerializedDiagnosticFix` to make it clear it's what is stored on `Diagnostic` or `SerializedDiagnosticData`. I mainly want to point out that I'm overwhelmed by this and struggle to understand the different data structures.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:121 on 2024-03-26 17:57_

Nit: I would remove the generic type considering that `edits` is always a slice
```suggestion
fn quick_fix<'d>(
    edits: &['d DiagnosticEdit],
) -> impl Iterator<Item = CodeActionOrCommand> + 'd {
```

It simplifies the signature a great deal

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:86 on 2024-03-26 17:57_

Nit: I wonder if we could use `take` here and rename `original_diagnostic` to `fixed_diagnostic` to make it clear it's not the "entire" diagnostic anymore.

---

_@MichaReiser approved on 2024-03-26 18:00_

Nice, now we have almost feature parity with the old extension for Pyton files. 

This looks good to me. 

I have a few small nits. Feel free to disregard them; this is mainly a matter of personal preference. My main feedback is that I recommend avoiding `impl` when we only call the methods with a single type (same for return types). I do find `impl` types more difficult to understand and oftentimes require explicit lifetimes where they aren't required when using an explicit type

I do think there's an opportunity to improve the naming. It took me a while to understand all the different types involved. I made a suggestion inline. 

We should make sure that the LSP also supports the ruff specific `fixAll` action, see my other inline comment for the details.

Once you add the test plan to the PR (which you planned on doing anyway), this is good to go. 



---

_@snowsignal reviewed on 2024-03-26 21:56_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action.rs`:86 on 2024-03-26 21:56_

My only concern is that this would make the list of 'fixed' diagnostics that we provide (which hints at the diagnostics that get fixed with a code action) no longer perfectly match the original diagnostics the client has, since they'll be missing the `data` field. Unfortunately the specification isn't clear whether it has to be an exact match or whether it matches on certain fields. Since this is just supposed to be a hint though, it's probably fine.

---

_@snowsignal reviewed on 2024-03-26 23:06_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action.rs`:52 on 2024-03-26 23:06_

I've made renamings similar to what you suggested, with one exception: I changed `DiagnosticFix` to `AssociatedDiagnosticData` because I think that describes what its actual role is as a serialized structure. `SerializedFix` may imply that it contains serialized values, which it doesn't.

I also reworked `DiagnosticEdit` (now `DiagnosticFix`) to only include the relevant fields from `AssociatedDiagnosticData` so it's a bit less confusing.

---

_Comment by @snowsignal on 2024-03-26 23:10_

I've addressed all the suggestions and rebased against `main` to get the fix for https://github.com/astral-sh/ruff/issues/10618. All that's left is the test plan video.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:199 on 2024-03-27 04:48_

We will need to advertise to the client that we support Ruff scoped code actions during initialization. Refer https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L852-L869

---

_@dhruvmanila reviewed on 2024-03-27 04:58_

---

_@MichaReiser reviewed on 2024-03-27 08:19_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:86 on 2024-03-27 08:19_

Yeah. That's why I'm unsure if it's okay to do so. I'm very surprised that the response must contain the entire diagnostic and not just an ID. 

---

_Comment by @MichaReiser on 2024-03-27 08:22_

Thanks for addressing all the feedback.

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-03-28 08:10_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/lint.rs`:100 on 2024-03-28 08:26_

nit: this might just be a personal preference but I find it harder to read fully qualified path when the type itself should be sufficient. For example, `DocumentVersion` instead of `crate::edit::DocumentVersion`. But, it's up to you if you find having this easier

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:11 on 2024-03-28 08:41_

Can you expand on why this is called "extension" capabilities? Which extension does this refer to? I think these are the extracted client capabilities to be queried at a later point. Maybe we can use `ClientCapabilities` (conflicting) / `ResolvedClientCapabilities` / `SupportedClientCapabilities` with `support_code_action_resolve` method?

```rs
#[derive(Debug, Clone, PartialEq, Eq, Default)]
pub(crate) struct ResolvedClientCapabilities {
    code_action_resolve: bool,
}

impl ResolvedClientCapabilities {
	pub(crate) fn supports_code_action_resolve(&self) -> bool {
		self.code_action_resolve
	}
}

impl From<&ClientCapabilities> for ResolvedClientCapabilities {
	fn from(client_capabilities: &ClientCapabilities) -> Self {
		// ...
	}
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/settings.rs`:6 on 2024-03-28 08:43_

Can you add please some documentation on what these server settings are? What is the intended use case of them and what should be included? To me it seems they're settings to configure the Ruff server but it contains capabilities which I find a bit confusing.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server.rs`:272 on 2024-03-28 08:47_

nit: should we use `into_available` / `to_available` as per the convention? Or, we could even define a `From` implementation for this

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:76 on 2024-03-28 09:00_

Do you think we should just return an owned vector of fixes here instead of an iterator impl? Returning an impl Iterator makes me wonder if there's any potential use case, you will have more knowledge on this.

---

_@dhruvmanila reviewed on 2024-03-28 09:00_

This is great work!

I just have one recommendation which is to document the types and functions. This would be really helpful. It's does not need to be done in this PR itself.

I'm going to test this out locally in Neovim now and approve then.

---

_@dhruvmanila approved on 2024-03-28 09:13_

I just tested it in Neovim. It works really well.

I have noticed one minor thing but it could very well be something else. When I run the fixAll code action and then undo the edit via the editor, the cursor jumps to the first line of the file. This doesn't happen when I do so through `ruff-lsp`. This is a bit of inconvenience but I don't think it's related to your change. I can look into why this is happening. It could very well be an editor bug.

Here's a video showcasing that: the first code action applied is via `ruff-lsp` while the second one is the release build of this branch.

https://github.com/astral-sh/ruff/assets/67177269/c343e780-3d29-49c2-aabf-a17f7c1ecbb9



---

_@MichaReiser reviewed on 2024-03-28 09:40_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:6 on 2024-03-28 09:40_

I share the sentiment here, but I think it's mainly a naming issue than anything else:

* It's unexpected to find server settings in the session
* The settings (as they are today) configure only the client capabilities

I think my recommendation for today would be to

a) Remove `ServerSettings` and directly use `ClientCapabilities` or `ResolvedClientCapabilities` (inline today's `ExtensionCapabilities`). 
b) Rename to `SessionSettings`?

I would go with `a` and figure out the precise naming once we know what other kind of data settings store (should be an easy refactor)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:43 on 2024-03-28 09:48_

NIt
```suggestion
        debug_assert_eq!(
            available_action & (available_action - AvailableCodeActions::all()),
            AvailableCodeActions::empty()
        );
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:55 on 2024-03-28 09:48_

I think this needs an error text
```suggestion
            Err(anyhow::anyhow!(".....")).with_failure_code(ErrorCode::InvalidParams)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:47 on 2024-03-28 09:52_

What's the motivation here to call into `makes_available` over directly matching on the `SupportedCodeActionKind` considering that it should always be one action (you need to fall back to debug assertions to replace the static assertions when matching on `SupportedCodeActionsKind` direclty) 

```
match supported {
    SupportedCodeActionKind::SourceFixAll | SupportedCodeActionKind::SourceFixAllRuf => {

    }
    _ => {
        Err()
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:53 on 2024-03-28 09:59_


```suggestion
        let supported: SupportedCodeActionKind = action
            .kind
            .clone()
            .ok_or(anyhow::anyhow!("No kind was given for code action"))
            .with_failure_code(ErrorCode::InvalidParams)?
            .try_into()
            .map_err(|()| anyhow::anyhow!("Code action was of an invalid kind"))
            .with_failure_code(ErrorCode::InvalidParams)?;
        let available_action = supported.makes_available();

        // ensures that only one code action kind was made available
        debug_assert!(
            (available_action & (available_action - AvailableCodeActions::all()))
                == AvailableCodeActions::empty()
        );

        if available_action == AvailableCodeActions::SOURCE_FIX_ALL {
            resolve_edit_for_fix_all(
                action,
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:79 on 2024-03-28 10:00_

Would it be sufficient to only store the client capabilities on the session, or why do we need to store it on the sessoin and server?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/settings.rs`:30 on 2024-03-28 10:01_

We also need to check that the extension has data support:

> This is also guarded by an additional client capability codeAction.dataSupport. In general, a client should offer data support if it offers resolve support. It should also be noted that servers shouldn’t alter existing attributes of a code action in a codeAction/resolve request.

---

_@MichaReiser requested changes on 2024-03-28 10:03_

I've been a bit surprised by the amount of changes in your last commit.  Were the changes necessary to address a bug in the implementation? 

If not, then I would recommend for the future to merge the existing PR (it's functional) and create a new PR with the changes instead. This has a few advantages in my view:

* It gives you place to explain and motivate your change. This prevents the situation I'm in now where it's unclear why we implemented a new resolve handler. What is it for, what does it enable? Why the added complexity? 
* It avoids that reviewers have to re-review the entire PR a second time. Instead, they can focus on the latest changes (in context of what you summarized)


If you make such fundamental changes to existing PRs (because it's required to address a bug), then I recommend you to explicitly re-request review. It was unclear to me why the PR wasn't merged this morning and I only understood why after seeing @dhurvmanila's comments

Would you mind explaining the most recent changes in the PR summary. How does `resolve` and code action work together. How do we handle clients that don't support these client capabilities. I'm sure I could read this out from the code but that adds an extra burden to the reviewer because:

* I have to figure out the actual behavior from code
* I have to figure out the expected behavior my self (or the behavior you intended)


That makes it more difficult for me to make a useful review. 

---

_Comment by @snowsignal on 2024-03-28 23:58_

@MichaReiser My apologies for not explaining the reasons I made these changes more clearly. I'll try to go over the issue with this PR and the need to implement `codeAction/resolve` as a part of that:

1. When we generate a list of code actions, we're given a list of diagnostics to generate those code actions for. My first pass implementation made the assumption that this diagnostic list would be for the entire file, and so the fixes used to create the `fixAll` workspace edit were taken directly from the diagnostics provided.
2. However, this isn't how the code action endpoint works, most of the time. The client usually asks for code actions based on diagnostics in a specific range, which for VS Code is usually just the underlined diagnostic the cursor is pointing to. 
3. The way the existing LSP (`ruff-lsp`) solves this issue is by always making `source.fixAll` (and `source.organizeImports`) available as a code action, and attempting to defer calculating the `edit` field until the source action is actually being called.
4. That's essentially what I changed this PR to do - we check if the client supports deferred code action resolution, and defer calculating the diagnostic for the entire file. If the client supports this deferred resolution, we set the `edit` field to `None` when building the code action as part of the `textDocument/codeAction` request. It's then the client's job to call `codeAction/resolve` once the code action is about to be used, where we populate the `edit` field with the edits to make, if safe fixes exist for the file. If the client doesn't support deferred resolution, we populate the `edit` field during the `textDocument/codeAction` request.

I hope that clears up the confusion around these changes. I'll try to have these explanations prepared in advance for the future.

---

_@snowsignal reviewed on 2024-03-29 00:00_

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:100 on 2024-03-29 00:00_

I generally like to keep `use` statements to a minimum, but I can agree it's a bit verbose. I'll try to remove fully qualified paths where the type in question has an obvious origin.

---

_@snowsignal reviewed on 2024-03-29 00:05_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:11 on 2024-03-29 00:05_

My intent for `ServerSettings` and its sub-structure `ExtensionCapabilities` was to reflect the configuration being set on the client side (AKA the 'editor extension'), along with the capabilities of the client. That being said, `ResolvedClientCapabilities` is probably a better name for this!

---

_@snowsignal reviewed on 2024-03-29 00:07_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:272 on 2024-03-29 00:07_

We take `self` by value here because `Self` is `Copy`-able and clippy complains if you take it by reference. It's not meant to be a conversion method, but maybe that needs to be clarified.

---

_@snowsignal reviewed on 2024-03-29 00:14_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:76 on 2024-03-29 00:14_

Returning an `impl Iterator` tends to be my default when writing functions that use iterators (as opposed to collecting them preemptively) because it gives the caller more control over the allocation and execution of the iterator. I'm not opposed to `.collect()` in a function body if it makes the code easier to read though, so I'm fine with changing the signature.

---

_@snowsignal reviewed on 2024-03-29 00:21_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:47 on 2024-03-29 00:21_

I wanted to avoid duplicating the logic of `makes_available`, since I was concerned about a misalignment in logic if we added new variants of code action kinds to be accounted for. However, I'd be fine with using a match statement like this if we make sure it's exhaustive, so that adding new variants would force us to update the `match` logic.

---

_Review comment by @snowsignal on `crates/ruff_server/src/server.rs`:79 on 2024-03-29 00:25_

We need a separate reference to `client_capabilities` to pass into `Server::try_register_capabilities` because the session is already borrowed mutably by the scheduler.

---

_@snowsignal reviewed on 2024-03-29 00:25_

---

_@snowsignal reviewed on 2024-03-29 00:28_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:53 on 2024-03-29 00:28_

I'm not sure what this suggestion is for - it just seems to be removing most of the `resolve_edit_for_fix_all` call, which wouldn't compile.

---

_Review requested from @MichaReiser by @snowsignal on 2024-03-29 05:16_

---

_Comment by @MichaReiser on 2024-03-29 07:55_

Thanks for the explanation. That's helfpul context for reviewing the PR

> If the client doesn't support deferred resolution, we populate the edit field during the textDocument/codeAction request.

Does that mean that `fix all` for clients not supporting `resolve` doesn't fix all diagnostics but only the diagnostics in view (the trimmed-down diagnostics selected by the editor?)

---

_@MichaReiser reviewed on 2024-03-29 07:57_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:53 on 2024-03-29 07:57_

Hmm yeah, this is nonsense. Sorry. I'm still struggling with the multiline suggestion feature and must have incorrectly submitted this suggestion.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:86 on 2024-03-29 08:00_

Nit: 
```suggestion
        // The editor will request the edit in a `CodeActionsResolve` request
        edit: None,
```

---

_Comment by @snowsignal on 2024-03-29 08:00_

> Does that mean that fix all for clients not supporting resolve doesn't fix all diagnostics but only the diagnostics in view (the trimmed-down diagnostics selected by the editor?)

I should have been more clear on that: we _do_ fix all diagnostics, it's just less performant because we have to lint the file right then instead of deferring that to when it's actually used.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:107 on 2024-03-29 08:04_

Nit: I found it a bit confusing that we initialize `action` only to override it then. It seems the motivation for doing this is so that the `action` can be passed to `resolve_edit_for_fix_all`, which sets `action.edit`. I think I would restructure this to
```suggestion
    let (edit, data) = if snapshot.resolved_client_capabilities().code_action_deferred_edit_resolution {
        // This will be resolved later
        (None, Some(serde_json::to_value(snapshot.url()).expect("document url to serialize")))
    } else {
        (resolve_edit_for_fix_all(document, snapshot.url(), &snapshot.configuration.linter, snapshot.encoding()), None)
    };

    let action = types::CodeAction {
        title: format!("{DIAGNOSTIC_NAME}: Fix all auto-fixable problems"),
        kind: Some(types::CodeActionKind::SOURCE_FIX_ALL),
        edit: None,
        data,
        ..Default::default()
    };

    Ok(types::CodeActionOrCommand::CodeAction(action))
```

This way it's clearer what's different between the two paths and it avoids setting `data` for clients that don't support it.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:14 on 2024-03-29 08:08_

Nit: It might be helpful to document the different code actions. What are they supposed to do and how do they work with other code actions.

---

_@MichaReiser approved on 2024-03-29 08:14_

---

_@dhruvmanila reviewed on 2024-03-29 11:09_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action.rs`:31 on 2024-03-29 11:09_

Did this get accidentally checked-in or is it suppose to be at a different log level?

---

_@snowsignal reviewed on 2024-03-29 14:45_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action.rs`:31 on 2024-03-29 14:45_

This was checked in by accident, my bad.

---

_Comment by @snowsignal on 2024-04-02 20:58_

I've made one final major refactor (which I _hope_ will be the last) in https://github.com/astral-sh/ruff/pull/10597/commits/de6870df6528455b59adbc10cfe8247cac449e78.

Up until now, we've been building a workspace edit for `source.fixAll` by aggregating fixes from individual diagnostics (which are taken from `ruff_linter::linter::check_path`). However, if these diagnostic fixes cause conflicting changes or result in additional diagnostics being surfaced, aggregating them is actually the wrong move. What we should be doing is incrementally applying fixes until we've reached convergence, which is what `ruff_linter::linter::lint_fix` does. So now, when resolving the workspace edit for `source.fixAll`, we call `lint_fix` to get a fixed source file, and then we use `Replacement::between` to create a diff between the original source and the transformed source, which is used as the workspace edit.

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-02 20:59_

---

_Comment by @MichaReiser on 2024-04-03 06:39_

Whoops. I directly commented on the commit https://github.com/astral-sh/ruff/commit/de6870df6528455b59adbc10cfe8247cac449e78 because that was the easiest to review the changes but it seems GitHub doesn't show these comments :( Anyway. Looks good to me. 

---

_@dhruvmanila approved on 2024-04-03 07:10_

---

_Comment by @dhruvmanila on 2024-04-03 07:11_

> Whoops. I directly commented on the commit [de6870d](https://github.com/astral-sh/ruff/commit/de6870df6528455b59adbc10cfe8247cac449e78) because that was the easiest to review the changes but it seems GitHub doesn't show these comments :( Anyway. Looks good to me.

I think that only works if you open the commit from the PR's "Files Changed" tab. There's a commit filter on the top of the diff view. Maybe that should render the comments correctly.

---

_Merged by @snowsignal on 2024-04-03 16:22_

---

_Closed by @snowsignal on 2024-04-03 16:22_

---

_Branch deleted on 2024-04-03 16:22_

---
