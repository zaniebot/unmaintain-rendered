```yaml
number: 1717
title: "Zed: Unclear where to find the logs when ty crashes"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-12-02T10:29:38Z
updated_at: 2026-01-12T09:32:49Z
url: https://github.com/astral-sh/ty/issues/1717
synced_at: 2026-01-12T09:56:37Z
```

# Zed: Unclear where to find the logs when ty crashes

---

_Issue opened by @MichaReiser on 2025-12-02 10:29_

<img width="493" height="68" alt="Image" src="https://github.com/user-attachments/assets/72b314e1-ec03-4a1b-b2bf-ef6c4884a946" />

It's unclear to users where to find the logs. 

---

_Label `server` added by @MichaReiser on 2025-12-02 10:29_

---

_Comment by @MatthewMckee4 on 2025-12-02 11:15_

It seems like there's not a canonical way to tell what editor someone is using?

Though, for zed, there are some environment variables we can check if set: `ZED_ENVIRONMENT`.

---

_Comment by @MichaReiser on 2025-12-02 11:50_

One option is to handle this in Zed's extension. Another is to try to detect the Zed client by inspecting the client name (part of the initialization request) or rely on environment variables as you suggest

---

_Comment by @dhruvmanila on 2026-01-12 09:32_

https://github.com/astral-sh/ruff/pull/22530 could help?

---
