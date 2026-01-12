```yaml
number: 2259
title: Failed to start in Vscode
type: issue
state: closed
author: river-walras
labels: []
assignees: []
created_at: 2025-12-29T15:26:30Z
updated_at: 2025-12-29T18:17:02Z
url: https://github.com/astral-sh/ty/issues/2259
synced_at: 2026-01-12T15:54:26Z
```

# Failed to start in Vscode

---

_@river-walras_

### Summary

2025-12-29 15:25:40.404729434  INFO Version: 0.0.8
2025-12-29 15:25:40.435629604 ERROR Got an error from the client (code -32603, method workspace/configuration): Request workspace/configuration failed with message: Cannot destructure property 'version' of 'l[e]' as it is undefined.

### Version

ty 0.0.8

---

_Comment by @MichaReiser on 2025-12-29 15:31_

Sorry that you ran into this. Can you try updating the extension to `2025.80.0`. It was published only a few mintues ago and should fix this issue. You might need to restart VS Code a few times to make it pick up on the newly released extension.

---

_Closed by @MichaReiser on 2025-12-29 18:17_

---
