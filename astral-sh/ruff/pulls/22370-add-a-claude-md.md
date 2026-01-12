```yaml
number: 22370
title: Add a CLAUDE.md
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/claude
created_at: 2026-01-04T18:44:50Z
updated_at: 2026-01-04T20:00:33Z
url: https://github.com/astral-sh/ruff/pull/22370
synced_at: 2026-01-12T15:57:48Z
```

# Add a CLAUDE.md

---

_@charliermarsh_

## Summary

This is a starting point based on my own experiments. Feedback and changes welcome -- I think we should iterate on this a lot as we go.


---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-04 18:46_

---

_Review requested from @zanieb by @charliermarsh on 2026-01-04 18:46_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-04 18:46_

---

_Label `internal` added by @charliermarsh on 2026-01-04 18:46_

---

_@AlexWaygood reviewed on 2026-01-04 18:55_

---

_Review comment by @AlexWaygood on `CLAUDE.md`:56 on 2026-01-04 18:55_

I don't think I actually have pre-commit (or anything, really) installed globally anymore, I just always run this instead:

```suggestion
- Always run `uvx pre-commit run -a` at the end of a task.
```

---

_@MichaReiser reviewed on 2026-01-04 19:36_

---

_Review comment by @MichaReiser on `CLAUDE.md`:10 on 2026-01-04 19:36_

I find using nextest is faster but I also enabled the developer exclusion for my shells

---

_Review comment by @AlexWaygood on `CLAUDE.md`:5 on 2026-01-04 19:38_

Should we also say that you'll often need to run `cargo insta review` to update snapshots after running the tests?

---

_@charliermarsh reviewed on 2026-01-04 19:41_

---

_Review comment by @charliermarsh on `CLAUDE.md`:10 on 2026-01-04 19:41_

Can you include the invocations you typically use (e.g. for a specific test or file)?

---

_@AlexWaygood reviewed on 2026-01-04 19:41_

---

_@charliermarsh reviewed on 2026-01-04 19:42_

---

_Review comment by @charliermarsh on `CLAUDE.md`:5 on 2026-01-04 19:42_

I can't remember if Claude can actually run this, it might need `cargo insta accept`?

---

_Marked ready for review by @charliermarsh on 2026-01-04 19:42_

---

_@AlexWaygood reviewed on 2026-01-04 19:43_

---

_Review comment by @AlexWaygood on `CLAUDE.md`:5 on 2026-01-04 19:43_

hmm, yeah

---

_Review comment by @MichaReiser on `CLAUDE.md`:10 on 2026-01-04 19:44_

It's the same. The only difference is the `run` subcommand: `cargo nextest run`. At least, I'm not aware of any other difference

---

_@MichaReiser reviewed on 2026-01-04 19:44_

---

_@MichaReiser approved on 2026-01-04 19:54_

---

_Merged by @charliermarsh on 2026-01-04 20:00_

---

_Closed by @charliermarsh on 2026-01-04 20:00_

---

_Branch deleted on 2026-01-04 20:00_

---
