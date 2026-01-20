```yaml
number: 22636
title: Mechanism to opt-in or opt-out of markdown formatting
type: issue
state: open
author: amyreese
labels: []
assignees: []
created_at: 2026-01-16T23:26:57Z
updated_at: 2026-01-20T07:12:59Z
url: https://github.com/astral-sh/ruff/issues/22636
synced_at: 2026-01-20T07:37:37Z
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

_Comment by @MichaReiser on 2026-01-20 07:12_

Ah sorry. I didn't see your comment here before posting on the PR.

I don't know. But using `include`/`exclude` is definitely the first thing that comes to mind when it's about defining on which files Ruff should run. I don't have the answer here but we should explore the different options and then decide on a design


---
