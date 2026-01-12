```yaml
number: 12709
title: "Server setting to run subset of rules for \"Fix All\" code action"
type: issue
state: open
author: dhruvmanila
labels:
  - configuration
  - server
  - needs-design
assignees: []
created_at: 2024-08-06T06:07:33Z
updated_at: 2025-03-03T10:45:23Z
url: https://github.com/astral-sh/ruff/issues/12709
synced_at: 2026-01-12T15:54:52Z
```

# Server setting to run subset of rules for "Fix All" code action

---

_@dhruvmanila_

Discussion: https://github.com/astral-sh/ruff/pull/12674

It would be useful to provide a server (and VS Code extension) setting to allow including / excluding only a subset of rules to be fixed when running the `source.fixAll.ruff` code actions on save. 

### Notes

* It's important that this is only considered when running the code action on save and not when running it manually. This is present in the request payload under the `context` field as [`CodeActionTriggerKind`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#codeActionTriggerKind).
* This is a filter which is applied on top of the rule selection as done via a config file / server setting.

For prior art, refer to `eslint.codeActionsOnSave.rules`. This allows for patterns and allows negation as well. Search for "eslint.codeActionsOnSave.rules" in https://github.com/microsoft/vscode-eslint#settings-options for examples.

We could introduce a similar option `ruff.codeActionsOnSave.rules` but I don't think we should allow for patterns in it. Our rule selection allows for shorter codes that negates the need for `*` pattern. But, we do need to consider about a config to exclude rules instead of including them.

One option would be to provide both `include` and `exclude` field like so:

```json
{
	"ruff.codeActionsOnSave.rules": {
		"include": [],
		"exclude": [],
	}
}
```

Or, allow a negation pattern using `!`:

```json
{
	"ruff.codeActionsOnSave.rules": [
		"!F401",
	]
}
```

---

_Label `configuration` added by @dhruvmanila on 2024-08-06 06:07_

---

_Label `server` added by @dhruvmanila on 2024-08-06 06:07_

---

_Label `needs-design` added by @dhruvmanila on 2024-08-06 06:07_

---

_Comment by @dhruvmanila on 2025-02-11 08:52_

Cross linking from the discussion thread: https://github.com/astral-sh/ruff/discussions/15991#discussioncomment-12127123

---

_Comment by @dhruvmanila on 2025-03-03 06:51_

The latest version of Ruff (`0.9.8`) expanded `ruff.configuration` to accept a JSON object representing the Ruff config. Read more about it in the docs: https://docs.astral.sh/ruff/editors/settings/#configuration.

---

Cross posting from https://github.com/astral-sh/ruff-vscode/issues/260#issuecomment-2689725523

I think `ruff.configuration` can partially resolve this issue i.e., a user can configure Ruff in an editor context to mark certain rules as `unfixable` which will then be considered when applying code actions on save. But, it'll also mean that those won't show up in the lightbulb menu nor as a "Quick fix" in the diagnostic popup.

Consider this example config:
```json
{
  "ruff.configuration": {
    "lint": {
      "unfixable": ["F541"]
    }
  }
}
```

But, this might still be useful to provide so that the user still have the option to fix it if they want to.

---

_Comment by @T-256 on 2025-03-03 10:45_

A simpler proposal from https://github.com/astral-sh/ruff-vscode/issues/260#issuecomment-2692187606 which then will apply configuration for specific code actions.
For example, `source.fixAll`:
```json
"editor.codeActionsOnSave": {
    "source.fixAll": "explicit"
},

                 // also: organizeImports/format
"ruff.configuration.fixAll": {
    "lint": {
      "unfixable": ["F541"]
    }
}
```

---
