```yaml
number: 1582
title: Improve and test diagnostic rendering in editors that can show annotations
type: issue
state: closed
author: sharkdp
labels:
  - diagnostics
  - server
assignees: []
created_at: 2025-11-18T13:57:08Z
updated_at: 2025-12-09T12:18:32Z
url: https://github.com/astral-sh/ty/issues/1582
synced_at: 2026-01-12T15:54:25Z
```

# Improve and test diagnostic rendering in editors that can show annotations

---

_@sharkdp_

In IDEs that can display annotations, we currently show a mixture of "concise" and "full" diagnostic information. This can lead to duplication of information:

<img width="1041" height="475" alt="Image" src="https://github.com/user-attachments/assets/b267d0ab-e08f-4e8d-9c97-556fc19220ab" />

<img width="1041" height="475" alt="Image" src="https://github.com/user-attachments/assets/2b565d3c-9b08-4c67-af15-d2604f760871" />

For editors that support this feature, we should switch to using the default primary message (in `to_lsp_diagnostic`, see description of [LSP capabilities](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticClientCapabilities)). Basically, we would do the same that we do on the terminal:

<img width="811" height="267" alt="Image" src="https://github.com/user-attachments/assets/c35473bc-ccda-4d8c-a178-cdc8cffb3c3e" />

Ideally, we would also add tests for this somewhere.

---

_Label `diagnostics` added by @sharkdp on 2025-11-18 13:57_

---

_Label `server` added by @carljm on 2025-11-18 17:17_

---

_Comment by @AlexWaygood on 2025-12-08 11:58_

Using the ty-vscode extension, I currently see some subdiagnostics listed twice, which I assume is caused by the same issue reported here. For example, hovering over `assert_never` here, I see the "Python 3.7 assumed due to this configuration setting" message twice:

<img width="3140" height="1020" alt="Image" src="https://github.com/user-attachments/assets/f18d6fe8-338b-440b-97b6-b659de0906e1" />

and the same thing if I open up the "Problems" tab:

<img width="2366" height="172" alt="Image" src="https://github.com/user-attachments/assets/7db6d4c0-3281-4502-b6fb-eef0651f1d9c" />

---

_Comment by @MichaReiser on 2025-12-08 13:38_

I think there are two issues:

The `unresolved-import` contains redundant information by saying that Python 3.7 was assumed twice:

<img width="970" height="305" alt="Image" src="https://github.com/user-attachments/assets/04640d63-d7d7-4a5c-8a01-09ddb3e17f06" />

This is also unnecessarily verbose on the CLI. 

I don't fully understand yet why there are two `Python 3.7` assumed sub diagnostics

---

_Closed by @MichaReiser on 2025-12-09 12:18_

---
