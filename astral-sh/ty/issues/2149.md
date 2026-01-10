---
number: 2149
title: Inconsistent semantic highlighting for imports in VS Code
type: issue
state: closed
author: GlobalMin
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-20T03:46:08Z
updated_at: 2025-12-24T17:25:31Z
url: https://github.com/astral-sh/ty/issues/2149
synced_at: 2026-01-10T01:52:52Z
---

# Inconsistent semantic highlighting for imports in VS Code

---

_Issue opened by @GlobalMin on 2025-12-20 03:46_

I'm very excited to be using `ty` given how well `uv` and `ruff` have changed my Python dev experience, and so far it's clearly very fast. The one issue I'm having in VSCode is some 3rd party libraries, meaning needing to install with `uv add` are not highlighted like others and it looks off. See below for example where `pandas` is colored white while the other import paths are in green. Not all external libs have issues as shown in screenshot as well - `fastapi` looks fine.

<img width="459" height="271" alt="Image" src="https://github.com/user-attachments/assets/81424b79-9139-496e-aaa5-f5045546a584" />

---

_Label `server` added by @AlexWaygood on 2025-12-21 20:23_

---

_Comment by @dhruvmanila on 2025-12-23 08:33_

I don't think it's related to third-party imports as it happens for standard library imports as well (https://play.ty.dev/2b5f19b2-59d7-496f-9b65-8ea19f7ee6ef).

I think it's because for an aliased import like `import foo as bar`, the server only sends the `Namespace` token for `bar` and not `foo` ([code ref](https://github.com/astral-sh/ruff/blob/4745d15fff5d73fe5a91cc9901df5ecf209ae60f/crates/ty_ide/src/semantic_tokens.rs#L708-L721)). Maybe we should send the `Namespace` token for `foo` as well?

It's interesting that I'm not seeing this discrepancy with the colorscheme that I'm using in VS Code (Gruvbox Material) but only with the default colorscheme.

---

_Renamed from "Inconsistent semantic highlighting for some third-party imports in VS Code" to "Inconsistent semantic highlighting for imports in VS Code" by @dhruvmanila on 2025-12-23 08:33_

---

_Comment by @MichaReiser on 2025-12-23 08:56_

I used `Developer: Inspect Editor Tokens and Scopes` to inspect the semantic tokens produced by pylance and it classifies both `pathlib` tokens as namespace

---

_Label `bug` added by @MichaReiser on 2025-12-23 08:56_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-23 08:56_

---

_Comment by @GlobalMin on 2025-12-23 17:45_

Ok I can confirm it's not about native Python versus 3rd party it's the `import a as b` pattern. Can reproduce 100% when adding imports like this.

---

_Closed by @MichaReiser on 2025-12-24 17:25_

---
