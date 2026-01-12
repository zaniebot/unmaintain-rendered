```yaml
number: 19107
title: "[ty] First cut at semantic token provider."
type: pull_request
state: closed
author: UnboundVariable
labels: []
assignees: []
base: main
head: semantic_tokens
created_at: 2025-07-02T22:28:09Z
updated_at: 2025-07-02T22:33:38Z
url: https://github.com/astral-sh/ruff/pull/19107
synced_at: 2026-01-12T15:56:32Z
```

# [ty] First cut at semantic token provider.

---

_@UnboundVariable_

This PR implements a basic semantic token provider for ty's language server. This allows for more accurate semantic highlighting / coloring within editors that support this LSP functionality.

Here are screen shots that show how code appears in VS Code using the "rainbow" theme both before and after this change.

![before](https://github.com/user-attachments/assets/15630625-d4a9-4ec5-9886-77b00eb7a41a)
![after](https://github.com/user-attachments/assets/d6dcf5f0-7b9b-47de-a410-e202c63e2058)

The token types and modifier tags in this implementation largely mirror those used in Microsoft's default language server for Python.

The implementation supports two LSP interfaces. The first provides semantic tokens for an entire document, and the second returns semantic tokens for a requested range within a document.

The PR includes unit tests. It includes comments that document known limitations and areas for future improvements.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-02 22:28_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-02 22:28_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-02 22:28_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-02 22:28_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-02 22:28_

---

_Comment by @github-actions[bot] on 2025-07-02 22:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pegen (https://github.com/we-like-parsers/pegen)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~41MB

aioredis (https://github.com/aio-libs/aioredis)
-     memo fields = ~54MB
+     memo fields = ~60MB

operator (https://github.com/canonical/operator)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB
-     memo fields = ~88MB
+     memo fields = ~80MB

werkzeug (https://github.com/pallets/werkzeug)
-     memo fields = ~142MB
+     memo fields = ~129MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~334MB
+ TOTAL MEMORY USAGE: ~368MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

```
</details>


---

_Closed by @UnboundVariable on 2025-07-02 22:33_

---

_Branch deleted on 2025-07-02 22:33_

---
