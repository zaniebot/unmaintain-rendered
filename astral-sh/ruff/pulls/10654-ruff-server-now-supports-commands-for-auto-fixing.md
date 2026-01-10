```yaml
number: 10654
title: "`ruff server` now supports commands for auto-fixing, organizing imports, and formatting"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/commands
created_at: 2024-03-29T08:04:47Z
updated_at: 2024-04-06T02:39:51Z
url: https://github.com/astral-sh/ruff/pull/10654
synced_at: 2026-01-10T22:47:02Z
```

# `ruff server` now supports commands for auto-fixing, organizing imports, and formatting

---

_Pull request opened by @snowsignal on 2024-03-29 08:04_

## Summary

This builds off of the work in https://github.com/astral-sh/ruff/pull/10652 to implement a command executor, backwards compatible with the commands from the previous LSP (`ruff.applyAutofix`, `ruff.applyFormat` and `ruff.applyOrganizeImports`). 

This involved a lot of refactoring and tweaks to the code action resolution code - the most notable change is that workspace edits are specified in a slightly different way, using the more general `changes` field instead of the `document_changes` field (which isn't supported on all LSP clients). Additionally, the API for synchronous request handlers has been updated to include access to the `Requester`, which we use to send a `workspace/applyEdit` request to the client.

## Test Plan


https://github.com/astral-sh/ruff/assets/19577865/7932e30f-d944-4e35-b828-1d81aa56c087




---

_Label `server` added by @snowsignal on 2024-03-29 08:04_

---

_Comment by @snowsignal on 2024-03-29 08:05_

(This is in draft because I'd like to hold off reviews until the downstream PRs are merged in, and also because I still need to make a test plan video for this)

---

_Comment by @github-actions[bot] on 2024-03-29 08:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-03-29 08:38_

Sorry for jumping in on a Draft PR. Before publishing, would you mind explaining what happens for editors not supporting the `changes` field?

> the most notable change is that workspace edits are specified in a slightly different way, using the more general changes field instead of the document_changes field (which isn't supported on all LSP clients).

---

_Comment by @snowsignal on 2024-03-29 09:55_

@MichaReiser:
> Sorry for jumping in on a Draft PR. Before publishing, would you mind explaining what happens for editors not supporting the `changes` field?

It's the `document_changes` field that isn't supported on all clients. `changes` should have universal support.

From the [LSP specification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspaceEdit):
```
If a client neither supports `documentChanges` nor `workspace.workspaceEdit.resourceOperations`,
then only plain `TextEdit`s using the `changes` property are supported.
```

---

_Marked ready for review by @snowsignal on 2024-04-04 23:53_

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-04 23:55_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-04 23:55_

---

_Comment by @MichaReiser on 2024-04-05 06:59_

Would you mind explaining the motivation for migrating from `documentChanges` to `changes`?

> This involved a lot of refactoring and tweaks to the code action resolution code - the most notable change is that workspace edits are specified in a slightly different way, using the more general changes field instead of the document_changes field (which isn't supported on all LSP clients). 

From the LSP specification

> A workspace edit represents changes to many resources managed in the workspace. The edit should either provide changes or documentChanges. If the client can handle versioned document edits and if documentChanges are present, the latter are preferred over changes.

This makes me think that we should use `documentChanges` whenever possible (probably because of the verisoning)

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:46 on 2024-04-05 07:03_

Nit: We could incline `args_as_text_documents` to avoid the vec allocation and without increasing the complexity (I almost find it easier to read because I don't need to jump to `args_as_text_documents` to understand what's happening)
```suggestion
        for argument in params.arguments {
            let document = serde_json::from_value(argument).with_failure_code(ErrorCode::InvalidParams)?;
            let snapshot = session
                .take_snapshot(&document.uri)
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:104 on 2024-04-05 07:05_

What's the motivation for testing this at the very end? Should we instead not promote the server capability (or commands) if the client doesn't support `workspace/applyEdit`? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:88 on 2024-04-05 07:09_

Hmm, this could get interesting once we have fixes that can "fix" across file because it is then no longer guaranteed that the changes are not overlapping. But that's something to worry about at another day. 

What we could consider doing is to move this inside the loop?

---

_@MichaReiser reviewed on 2024-04-05 07:15_

Thanks for the extensive test plan. 

This looks good code wise. I'm not sure about migrating from `documentChanges` to `changes`. I would like to understand the motivation/reason for this better.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:15 on 2024-04-05 09:01_

nit: It's common to at least have the `Debug` trait derive on types.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:39 on 2024-04-05 09:08_

nit: we could possibly implement `FromStr` for `Command` and then use `?` operator to avoid creating an empty `anyhow` error. I'm not sure how will `with_failure_code` comes from which I believe you're aware of.

```rs
impl FromStr for Command {
    type Err = ();

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        Ok(match command {
            "ruff.applyAutofix" => Self::FixAll,
            "ruff.applyFormat" => Self::Format,
            "ruff.applyOrganizeImports" => Self::OrganizeImports,
            _ => return Err(()),
        })
    }
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2024-04-05 09:12_

Can you confirm whether `changes` support having multiple documents? I can't exactly figure out reading the spec but I presume it does. But, if it doesn't then this will be problematic for Jupyter Notebooks where each cell is its own text document so the edits are tied to multiple documents.

---

_@dhruvmanila reviewed on 2024-04-05 09:15_

Thank you! I'll test this now in Neovim.

I'd also like to understand the migration from `document_changes` to `changes`. I've pointed out the reason for that.

Edit: I tested out in Neovim and it's working well.

---

_@dhruvmanila reviewed on 2024-04-05 09:16_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2024-04-05 09:16_

We could possibly test it out by creating a fake `changes` which have two entries with a single edit which just adds some text to those two files. If that works, then it's probably fine.

---

_@MichaReiser reviewed on 2024-04-05 10:01_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/execute_command.rs`:39 on 2024-04-05 10:01_

Good catch. I think we should always avoid anyhow error.

---

_@snowsignal reviewed on 2024-04-05 20:59_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:88 on 2024-04-05 20:59_

Multi-file fixes are going to require a lot of refactoring regardless, but yes, this is definitely something we'll need to solve eventually. For now though, it's a future concern.

---

_@snowsignal reviewed on 2024-04-05 21:00_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:104 on 2024-04-05 21:00_

Good idea, I'll move this up.

---

_Comment by @snowsignal on 2024-04-05 21:01_

The main reason I switched from `documentChanges` to `changes` was over concerns about supporting older versions. I think what I'll do is refactor our code to support `documentChanges` but use `changes` as a fallback.

---

_@snowsignal reviewed on 2024-04-05 21:05_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2024-04-05 21:05_

`changes` does enable multi-document edits, though none of the commands will modify multiple files. I think the LSP spec allows treating notebook files as a single document, so we could still have a single `changes` entry that covers multiple cells in a notebook.

---

_@snowsignal reviewed on 2024-04-05 21:07_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:39 on 2024-04-05 21:07_

We'd still need to define an error type for `FromStr` - I think using anyhow as an error makes sense here because it lets us pass an arbitrary error message. The message being empty here was just an oversight.

---

_Review requested from @charliermarsh by @snowsignal on 2024-04-05 21:53_

---

_@charliermarsh reviewed on 2024-04-05 22:20_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/execute_command.rs`:55 on 2024-04-05 22:20_

I would suggest perhaps passing in whatever's returned by `session.resolved_client_capabilities()` to reduce the risk of passing in a wrong boolean.

---

_@charliermarsh reviewed on 2024-04-05 22:22_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/code_action.rs`:63 on 2024-04-05 22:22_

Can we use `document_changes` here?

---

_@charliermarsh reviewed on 2024-04-05 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/execute_command.rs`:56 on 2024-04-05 22:28_

So is it guaranteed that documents aren't repeated here?

---

_@charliermarsh reviewed on 2024-04-05 22:29_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/execute_command.rs`:154 on 2024-04-05 22:29_

I naively would've assumed that this would _replace_ or _augment_ the existing edits, rather than erroring. I might suggest making it a replacement (`set_edits_for_document`)?

---

_@charliermarsh reviewed on 2024-04-05 22:29_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/execute_command.rs`:142 on 2024-04-05 22:29_

Do you mind adding a rustdoc for this one?

---

_@charliermarsh approved on 2024-04-05 22:35_

I would be pro-using `document_changes` everywhere, since that's what we do in the existing LSP and it's never been an issue. My _guess_ is that it's pretty rare for LSP's _not_ to support that (or, at least, that they must be very old), and I'm just not really clear on the implications of using `changes` in various places.

(I like that you made it a fallback in some places, but it looks like `quick_fix` at least still uses `changes` always.)

So I'd vote to continue using `document_changes` everywhere until it proves to be a problem. But it doesn't need to block this PR. You could also make that change (revert the use of `changes`) and merge without a re-review.

---

_@snowsignal reviewed on 2024-04-05 22:36_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:56 on 2024-04-05 22:36_

We enforce it indirectly by preventing re-setting edits on a document, but we should probably enforce it here too.

---

_@charliermarsh reviewed on 2024-04-05 22:37_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/server/api/requests/execute_command.rs`:154 on 2024-04-05 22:37_

It seems like one of the goals of `document_changes` is that it's _ok_ to repeat a URI as long as the version differs? So perhaps this check should be on URI and version?

---

_Comment by @snowsignal on 2024-04-05 22:43_

You're probably right that `changes` could be supported without issue. My general policy with features tied to client capabilities is that we _should_ have a fall-back in case that capability doesn't exist, but I should reconsider whether that makes sense going forward. I'm going to err on the side of backwards compatibility for now.

---

_Merged by @snowsignal on 2024-04-05 23:27_

---

_Closed by @snowsignal on 2024-04-05 23:27_

---

_Branch deleted on 2024-04-05 23:27_

---

_@snowsignal reviewed on 2024-04-06 01:17_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/execute_command.rs`:154 on 2024-04-06 01:17_

Still, I don't think it makes sense to apply document edits to the same document at different points in time simultaneously.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/execute_command.rs`:43 on 2024-04-06 02:39_

> I think the LSP spec allows treating notebook files as a single document, so we could still have a single `changes` entry that covers multiple cells in a notebook.

Yes, but it's not about a Notebook document here. How would the client know which _cell_ does this edit belong to? This is only known through the text document URI because each cell is represented using text document. Refer to [this loop](https://github.com/astral-sh/ruff-lsp/blob/187d7790be0783b9ac41ce025a724cf389bf575c/ruff_lsp/server.py#L1352-L1367) which does that in `ruff-lsp`.

---

_@dhruvmanila reviewed on 2024-04-06 02:39_

---
