```yaml
number: 2258
title: Language Server not loading from VSCode
type: issue
state: closed
author: gszzzzzz
labels:
  - server
assignees: []
created_at: 2025-12-29T14:58:47Z
updated_at: 2025-12-29T15:26:20Z
url: https://github.com/astral-sh/ty/issues/2258
synced_at: 2026-01-12T15:54:26Z
```

# Language Server not loading from VSCode

---

_@gszzzzzz_

### Summary

ty Language Server returning a following message:

```
2025-12-29 23:56:09.079798000  INFO Version: 0.0.8 (aa7559db8 2025-12-29)
2025-12-29 23:56:09.461645000 ERROR Got an error from the client (code -32603, method workspace/configuration): Request workspace/configuration failed with message: Cannot destructure property 'version' of 'l[e]' as it is undefined.
```

Reloading the language server didn't help either.

Project only had a single `main.py` file with `pyproject.toml`:
```
# pyproject.toml

dependencies = [
    "pydantic>=2.12.5",
    "requests>=2.32.5",
]

[dependency-groups]
dev = [
    "ty>=0.0.8",
]
```

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Comment by @gszzzzzz on 2025-12-29 14:59_

Note that manually rolling back version to `2025.76.0` fixed the problem.

---

_Label `server` added by @AlexWaygood on 2025-12-29 14:59_

---

_Comment by @MichaReiser on 2025-12-29 15:17_

Yes, sorry for this. This is the same as https://github.com/astral-sh/ty-vscode/issues/266 A patch release should be available very soon

---

_Comment by @MichaReiser on 2025-12-29 15:25_

I published `2025.80.0`, which should fix this issue. You might need to restart VS Code a few times to get it to pick up the newly released extension.

Let me know if you keep seeing this issue with 2025.80.0

---

_Closed by @MichaReiser on 2025-12-29 15:25_

---
