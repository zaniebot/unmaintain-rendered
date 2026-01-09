---
number: 14752
title: "`implicit-namespace-package` (`INP001`) - error is not reported in the language server when a package is nested inside an implicit namespace package"
type: issue
state: open
author: DetachHead
labels:
  - bug
  - server
assignees: []
created_at: 2024-12-03T10:53:59Z
updated_at: 2025-04-25T08:36:27Z
url: https://github.com/astral-sh/ruff/issues/14752
synced_at: 2026-01-07T13:12:16-06:00
---

# `implicit-namespace-package` (`INP001`) - error is not reported in the language server when a package is nested inside an implicit namespace package

---

_Issue opened by @DetachHead on 2024-12-03 10:53_

the scenario covered by #14236 seems to work in the CLI, but the error is not reported in vscode.

i'm not sure if this is an issue with the vscode extension or the language server, but the language server seems to not be sending any diagnostics so i assume that's where the issue is.

i'm using the same version of ruff in both the CLI and the vscode extension: 0.8.1

# project structure
```
foo/
├── __init__.py
└── bar/
    └── baz/
        └── __init__.py
```

# cli output
```
> ruff check --no-cache foo
foo\bar\baz\__init__.py:1:1: INP001 File `foo\bar\baz\__init__.py` declares a package, but is nested under an implicit namespace package. Add an `__init__.py` to `foo\bar`.
Found 1 error.
```

# vscode output
![Image](https://github.com/user-attachments/assets/50000174-b75a-422f-8899-17ac7716462b)
```
2024-12-03 20:47:55.022 [info] [Trace - 8:47:55 PM] Sending request 'textDocument/diagnostic - (175)'.
2024-12-03 20:47:55.022 [info] Params: {
    "identifier": "Ruff",
    "textDocument": {
        "uri": "file:///c%3A/Users/user/project/foo/bar/baz/__init__.py"
    }
}


2024-12-03 20:47:55.028 [info] [Trace - 8:47:55 PM] Received response 'textDocument/diagnostic - (175)' in 6ms.
2024-12-03 20:47:55.028 [info] Result: {
    "items": [],
    "kind": "full"
}
```

---

_Label `server` added by @AlexWaygood on 2024-12-03 11:33_

---

_Comment by @dhruvmanila on 2024-12-05 04:45_

Let me look into it. Can you provide the Ruff config file (if any) and the VS Code extension settings that's being used here?

---

_Comment by @dhruvmanila on 2024-12-05 04:51_

Yeah, this seems like a bug as we never account for the nested package root when checking from the server side:

https://github.com/astral-sh/ruff/blob/bd27bfab5d636cd5979ce78423ba069833ab13e2/crates/ruff_server/src/lint.rs#L86-L92

I'll need to look into the original change to understand where `PackageRoot::Nested` is being constructed.

---

_Label `bug` added by @dhruvmanila on 2024-12-05 04:51_

---

_Referenced in [astral-sh/ruff#15346](../../astral-sh/ruff/pulls/15346.md) on 2025-01-08 11:59_

---

_Comment by @maxromanovsky on 2025-04-25 08:36_

Faced lack of this check too when migrating from `flake8-no-pep420` to ruff.

---
