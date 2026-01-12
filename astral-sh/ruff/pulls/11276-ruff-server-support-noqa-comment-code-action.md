```yaml
number: 11276
title: "`ruff server`: Support `noqa` comment code action"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/noqa-code-action
created_at: 2024-05-04T01:33:06Z
updated_at: 2024-05-12T21:41:30Z
url: https://github.com/astral-sh/ruff/pull/11276
synced_at: 2026-01-12T15:55:37Z
```

# `ruff server`: Support `noqa` comment code action

---

_@snowsignal_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10594.

Code actions to disable a diagnostic via `noqa` comment are now available. 

https://github.com/astral-sh/ruff/assets/19577865/6d3bcf11-a9d9-499b-8c7f-a10cd39cfbba

`DiagnosticFix` has been changed so that `noqa` code actions appear even for diagnostics with no available quick fix. It can contain quick fix edits, `noqa` comment edits, or both.

## Test Plan

The scenarios that need to be tested are as follows:
* A code action to disable a diagnostic should be available for every diagnostic.
* Using this code action should append to the appropriate line with the diagnostic, or modify an existing `noqa` comment.
* Adding a `noqa` comment manually should make a diagnostic disappear
* `Fix all auto-fixable problems` should not add `noqa` comments
* Removing a code from a `noqa` comment should make the diagnostic re-appear


---

_Label `server` added by @snowsignal on 2024-05-04 01:33_

---

_Comment by @github-actions[bot] on 2024-05-04 01:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @T-256 on 2024-05-04 11:28_

![image](https://github.com/astral-sh/ruff/assets/132141463/538c9fc9-2d13-41ae-a895-45f4cf0f8834)

Could it possible instead of clearing noqa by hand, add code action when moving cursor on noqa code comments?
For example, moving cursor on `# noqa:`, show these code actions:
- Ruff (ANN201): Re-enable for this line.
- Ruff (D103): Re-enable for this line.
- Re-enable all disabled rules for this line.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:41 on 2024-05-06 10:30_

a) I think it could make sense to start documenting the fields. 

b) What's the motivation for using an `Option<Vec<_>>`? Could we just use an empty vec in case there are no edits? 

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:29 on 2024-05-06 10:34_

I think it's worthfile to start documenting the struct fields

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:84 on 2024-05-06 10:36_

I would use `filter_map` to avoid the `expect` call
```suggestion
        .filter_map(|fix| {
            let edits = fix.edits.as_ref()?;

            let mut tracker = WorkspaceEditTracker::new(snapshot.resolved_client_capabilities());

            tracker.set_edits_for_document(
                snapshot.url().clone(),
                document.version(),
                edits
            )?;

```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:106 on 2024-05-06 10:37_

Nit: I'm not sure if that works but maybe
```suggestion
            let edit = fix.noqa_edit.cloned()?;
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:85 on 2024-05-06 10:39_

Nit: I like to implement methods on structs that return references to remove the number of `as_ref` calls necessary:

```
fix.edits().expect()
```

---

_@MichaReiser approved on 2024-05-06 10:39_

---

_@snowsignal reviewed on 2024-05-07 06:32_

---

_Review comment by @snowsignal on `crates/ruff_server/src/lint.rs`:41 on 2024-05-07 06:32_

> b) What's the motivation for using an Option<Vec<_>>? Could we just use an empty vec in case there are no edits?

There is a distinction in the codebase between an empty vec and a `None` value (a fix with an empty vec would result in a code action that doesn't make any edits). But since 'a code action that doesn't make any edits' shouldn't be sent in the first place, I support your suggestion to make an empty vec represent the lack of a quick fix.

---

_Comment by @dhruvmanila on 2024-05-07 08:46_

Is this PR up-to date with the base PR (`jane/linter/noqa-edits`)? I think there's an issue with multiline strings:

Considering the above code snippet, in the following screen recording (left is new server, right is `ruff-lsp`) you can see that the new server adds the NoQA comment _inside_ the multiline string while `ruff-lsp` correctly adds it at the end.

https://github.com/astral-sh/ruff/assets/67177269/c75bb4dd-73d2-4c92-90fe-c3ec8268e138

---

_Comment by @snowsignal on 2024-05-07 19:12_

@dhruvmanila Thanks for pointing this out, I'll investigate.

---

_Review requested from @AlexWaygood by @snowsignal on 2024-05-09 19:03_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-05-09 19:03_

---

_Comment by @snowsignal on 2024-05-09 19:04_

@AlexWaygood @dhruvmanila You can ignore the review request, that seemed to happen automatically when I rebased the branch.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-05-09 19:12_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2024-05-09 19:12_

---

_Comment by @snowsignal on 2024-05-11 21:32_

@dhruvmanila I've confirmed that https://github.com/astral-sh/ruff/pull/11276/commits/b321a3fdbb3ef0c57d8029a80f461c282b10e1b0 fixes the issue with multiline diagnostics 👍 

---

_Merged by @snowsignal on 2024-05-12 21:39_

---

_Closed by @snowsignal on 2024-05-12 21:39_

---

_Branch deleted on 2024-05-12 21:39_

---

_Comment by @snowsignal on 2024-05-12 21:41_

@T-256 Thanks for this suggestion - can you open an issue for this?

---
