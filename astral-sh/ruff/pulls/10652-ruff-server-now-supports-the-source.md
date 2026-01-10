```yaml
number: 10652
title: "`ruff server` now supports the `source.organizeImports` source action"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/code-actions/organize-imports
created_at: 2024-03-29T05:46:54Z
updated_at: 2024-04-04T22:26:00Z
url: https://github.com/astral-sh/ruff/pull/10652
synced_at: 2026-01-10T22:47:02Z
```

# `ruff server` now supports the `source.organizeImports` source action

---

_Pull request opened by @snowsignal on 2024-03-29 05:46_

## Summary

This builds on top of the work in https://github.com/astral-sh/ruff/pull/10597 to support `Ruff: Organize imports` as an available source action.

To do this, we have to support `Clone`-ing for linter settings, since we need to modify them in place to select import-related diagnostics specifically (`I001` and `I002`).

## Test Plan

https://github.com/astral-sh/ruff/assets/19577865/04282d01-dfda-4ac5-aa8f-6a92d5f85bfd

---

_Label `server` added by @snowsignal on 2024-03-29 05:46_

---

_Comment by @github-actions[bot] on 2024-03-29 06:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @snowsignal on 2024-03-29 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:137 on 2024-03-29 08:29_

Nit: Same as for the `fix_all` pr. I think I would restructure it to avoid overriding action


```suggestion
    let (edit, data) = if snapshot.resolved_client_capabilities.code_action_deferred_edit_resolution {
        // The edit will be resolved later in the `CodeActionsResolve` request
        (None, Some(serde_json::to_value(snapshot.url()).expect("document url to serialize")))
    } else {
        (
            resolve_edit_for_organize_imports(
                document,
                snapshot.url(),
                snapshot.configuration().linter.clone(),
                snapshot.encoding(),
            )?,
            None
        )
    };

    let action = types::CodeAction {
        title: format!("{DIAGNOSTIC_NAME}: Organize imports"),
        kind: Some(types::CodeActionKind::SOURCE_ORGANIZE_IMPORTS),
        edit,
        data,
        ..Default::default()
    };

    Ok(types::CodeActionOrCommand::CodeAction(action))
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:97 on 2024-03-29 08:33_

I think you can reference the rule directly
```suggestion
        Rule::UnusedImport
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:93 on 2024-03-29 08:34_

Hmm, it's unfortunate that this requires making `LinterSettings` cloneable but I haven't been able to come up with an alternative solution that not at least requires cloning individual fields. Ideally it would be possible to call `check` with the desired rules, but that's a larger refactor and is probably not worth it.

I don't feel strongly but I have a slight preference to pass in a `LinterSetting` and move the cloning into `resolve_edit_for_organize_imports`.

---

_@MichaReiser approved on 2024-03-29 08:34_

---

_Comment by @MichaReiser on 2024-03-29 08:36_

Would you mind testing that adding the organizeImports action (ruff or the global one) into the on save action works as expected

```
{
  "[python]": {
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.organizeImports": "explicit"
    },
    "editor.defaultFormatter": "charliermarsh.ruff"
  }
}
```

(or using the ruff specific actions)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:93 on 2024-03-29 11:21_

Could we do something like the following?
1. Take a snapshot of existing settings
2. Mutate it temporarily
3. Restore the settings with the snapshot

I'm not sure how involved this is or if it's better than cloning. That said, I wouldn't recommend doing in this PR but something to think about if we need or want to refactor this in the future.

---

_@dhruvmanila reviewed on 2024-03-29 11:29_

---

_@snowsignal reviewed on 2024-04-04 21:50_

---

_Review comment by @snowsignal on `crates/ruff_server/src/server/api/requests/code_action_resolve.rs`:93 on 2024-04-04 21:50_

@MichaReiser @dhruvmanila I'll make a follow-up issue to look into how we could improve this for the future.

---

_Comment by @snowsignal on 2024-04-04 22:13_

@MichaReiser I've confirmed that the configuration you've provided works as expected. However, the ruff-specific configuration is not working, but for an unrelated reason, which I'll address in a follow-up issue.

---

_Merged by @snowsignal on 2024-04-04 22:20_

---

_Closed by @snowsignal on 2024-04-04 22:20_

---

_Branch deleted on 2024-04-04 22:20_

---
