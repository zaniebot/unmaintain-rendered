```yaml
number: 22636
title: Mechanism to opt-in or opt-out of markdown formatting
type: issue
state: open
author: amyreese
labels: []
assignees: []
created_at: 2026-01-16T23:26:57Z
updated_at: 2026-01-20T00:47:56Z
url: https://github.com/astral-sh/ruff/issues/22636
synced_at: 2026-01-20T01:38:21Z
```

# Mechanism to opt-in or opt-out of markdown formatting

---

_@amyreese_

_No description provided._

---

_Assigned to @amyreese by @amyreese on 2026-01-16 23:26_

---

_Comment by @amyreese on 2026-01-20 00:47_

Thinking about this some more (as I was adding a new option in #22750) should the "opt-in" for now simply be something like recommending `extend-include = ["**/*.md"]` and let `ruff format foo.md` just work even without needing `--preview` or a dedicated config option?

@MichaReiser @ntBre 

---
