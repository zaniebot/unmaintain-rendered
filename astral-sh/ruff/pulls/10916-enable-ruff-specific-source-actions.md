```yaml
number: 10916
title: Enable ruff-specific source actions
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: jane/server/ruff-source-actions
created_at: 2024-04-13T06:27:38Z
updated_at: 2024-04-16T18:26:23Z
url: https://github.com/astral-sh/ruff/pull/10916
synced_at: 2026-01-10T22:37:01Z
```

# Enable ruff-specific source actions

---

_Pull request opened by @snowsignal on 2024-04-13 06:27_

## Summary

Fixes #10780.

The server now send code actions to the client with a Ruff-specific kind, `source.*.ruff`. The kind filtering logic has also been reworked to support this.

## Test Plan

Add this to your `settings.json` in VS Code:

```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.organizeImports.ruff": "explicit",
    },
  }
}
```

Imports should be automatically organized when you manually save with `Ctrl/Cmd+S`.

---

_Label `bug` added by @snowsignal on 2024-04-13 06:27_

---

_Label `server` added by @snowsignal on 2024-04-13 06:27_

---

_Comment by @github-actions[bot] on 2024-04-13 06:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:117 on 2024-04-15 07:55_

Nit and unrelated to this PR: `kinds` could return a static slice:

```
    fn kinds(self) -> &'static [CodeActionKind] {
        match self {
            Self::QuickFix => {
                static QUICKFIX: [CodeActionKind; 1] = [CodeActionKind::QUICKFIX];
                &QUICKFIX
            }
            Self::SourceFixAll => {
                static FIXALL: [CodeActionKind; 2] =
                    [CodeActionKind::SOURCE_FIX_ALL, crate::SOURCE_FIX_ALL_RUFF];
                &FIXALL
            }
            Self::SourceOrganizeImports => {
                static ORGANIZE_IMPORTS: [CodeActionKind; 2] = [
                    CodeActionKind::SOURCE_ORGANIZE_IMPORTS,
                    crate::SOURCE_ORGANIZE_IMPORTS_RUFF,
                ];

                &ORGANIZE_IMPORTS
            }
        }
    }
```

---

_@MichaReiser reviewed on 2024-04-15 08:07_

It looks good. I would have found it useful if the PR summary contained a small description of how this PR fixes the issue. 

What happens if you put the following in your configuration

```
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.organizeImports": "explicit",
      "source.organizeImports.ruff": "explicit",
    },
  }
}
```

Is this the same way we promote both actions today? Could the LSP know which code actions the client enables to avoid generating unnecessary actions?

I looked at ESLint, and it seems they only generate the `source.fixAll.eslint` action (and not `source.fixAll`). I tried to test this locally and it seems to work (There's the chance that I messed up my testing setup but I'm somewhat confident):

VS code configurations:

```
  "editor.codeActionsOnSave": {
    "source.fixAll": "always"
  }
```

`fix_all`

```rust
    dbg!("Source fix all ruff only");
    Ok([SOURCE_FIX_ALL_RUFF]
        .into_iter()
        .map(move |kind| types::CodeAction {
            title: format!("{DIAGNOSTIC_NAME}: Fix all auto-fixable problems"),
            kind: Some(kind),
            edit: edit.clone(),
            data: data.clone(),
            ..Default::default()
        })
        .map(CodeActionOrCommand::CodeAction)
        .collect())
```

And VS code automatically applied the fixes. So maybe generating the fixes twice isn't necessary after all.

---

_Comment by @dhruvmanila on 2024-04-15 13:21_

(I was in the middle of writing the review comment when I pressed the back button which erased everything, I'll try my best to write everything again.)

I don’t think the server should return two code actions if the user has provided both `source.organizeImports` and `source.organizeImports.ruff`. The main reason for having a `.ruff` specific code action kinds is to allow users to explicitly mention that the client should only apply Ruff specific `organizeImports` action. For example, a user might have multiple language servers enabled for a specific language but only wants Ruff to apply the organize imports action.

Think of it more as a hierarchy where a more specific code action kind wins. Here’s a snippet from the LSP spec:

```jsx
/**
 * The kind of a code action.
 *
 * Kinds are a hierarchical list of identifiers separated by `.`,
 * e.g. `"refactor.extract.function"`.
 *
 * The set of kinds is open and client needs to announce the kinds it supports
 * to the server during initialization.
 */
export type CodeActionKind = string;
```

I’d also recommend testing this out with different values of `codeActionOnKind` which I believe are `explicit`, `always` , and `off` (confirm the values). What happens in a scenario when `source.organizeImports` is `explicit` while `source.organizeImports.ruff` is `always`. This might probably be handled by the editor but it would be useful to know the outcome of such scenarios.

I did a small test locally and saw a difference in the response between `ruff` and `ruff-lsp`. I’m not sure if it matters but something to check. If I ask only for `source.organizeImports.ruff` to both the server, the code action kind in the response is different: `ruff` gives `source.organizeImports` while `ruff-lsp` gives (possibly correct?) `source.organizeImports.ruff`.

---

_Comment by @snowsignal on 2024-04-16 04:14_

@MichaReiser @dhruvmanila I've tried to address your feedback as best as I could.

We now only return the ruff-specific code actions (`source.*.ruff`) in the code action response. I also reworked how we check supported code actions to account for this.

Here are the various configurations I've tested with the latest changes:

**Configuration**:
```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
    },
  }
}
```

**Result**: Manually saving automatically fixes all problems in the file.

**Configuration**:
```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": "explicit",
    },
  }
}
```

**Result**: Manually saving automatically fixes all problems in the file.

**Configuration**:
```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll": "never",
      "source.fixAll.ruff": "explicit"
    },
  }
}
```

**Result**: Manually saving automatically fixes all problems in the file.

**Configuration**: 
```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.fixAll.ruff": "explicit"
    },
  }
}
```

**Result**: Manually saving automatically fixes all problems in the file.

**Configuration**:
```json
{
  "[python]": {
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.fixAll.ruff": "never"
    },
  }
}
```

**Result**: Manual saving does nothing.

It's the same for `source.organizeImports`. Do these match what you expected? I think these behave the same as `ruff-lsp` except for the last one.

One strange thing I noticed is that the `always` value for the setting seemed to have no difference compared to `explicit` - that is, it didn't apply the code action on auto-saves, and you still had to save manually. This is also what `ruff-lsp` did, so I'm not sure whether this is the expected behavior or not.

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-16 04:14_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-16 04:14_

---

_Comment by @dhruvmanila on 2024-04-16 05:24_

> It's the same for `source.organizeImports`. Do these match what you expected? I think these behave the same as `ruff-lsp` except for the last one.

Yeah, I think the behavior in this PR is correct for the last example. I also confirmed with ESLint with the following VS Code settings:

```json
  "[javascript]": {
    "editor.codeActionsOnSave": {
      "source.fixAll": "explicit",
      "source.fixAll.eslint": "never"
    }
  }
```

And the following code:
```js
/* eslint curly: "error"*/

if (foo) foo++;
```

I think `ruff-lsp` must be doing an "or" check where if the kind is either of them, return the said code action.

---

_@dhruvmanila approved on 2024-04-16 05:51_

Thank you for addressing all the feedback and providing all of the test scenarios.

---

_Comment by @dhruvmanila on 2024-04-16 05:52_

Any reason why is this PR labeled as "bug"? I don't think there was support for it before this PR so I wouldn't classify it as a bug.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:121 on 2024-04-16 06:05_

The clones aren't necessary
```suggestion
        kind: Some(crate::SOURCE_FIX_ALL_RUFF),
        edit,
        data,
        ..Default::default()
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/code_action.rs`:156 on 2024-04-16 06:05_

The clones aren't necessary (weren't needed before)
```suggestion
        edit,
        data,
        ..Default::default()
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:299 on 2024-04-16 06:07_

Nit: I think I would return the owned value here (or remove the `static` aliases)
```suggestion
    fn to_kind(self) -> CodeActionKind {
        match self {
            Self::QuickFix => CodeActionKind::QUICKFIX,
            Self::SourceFixAll => crate::SOURCE_FIX_ALL_RUFF,
            Self::SourceOrganizeImports => crate::SOURCE_ORGANIZE_IMPORTS_RUFF,
        }
    }

```

---

_@MichaReiser approved on 2024-04-16 06:09_

---

_Comment by @snowsignal on 2024-04-16 18:09_

@dhruvmanila:
> Any reason why is this PR labeled as "bug"? I don't think there was support for it before this PR so I wouldn't classify it as a bug.

The original issue that this PR solves was that we _should_ have supported this but that support was broken.

---

_Merged by @snowsignal on 2024-04-16 18:21_

---

_Closed by @snowsignal on 2024-04-16 18:21_

---

_Branch deleted on 2024-04-16 18:21_

---
