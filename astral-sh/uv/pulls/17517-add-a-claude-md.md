```yaml
number: 17517
title: "Add a `CLAUDE.md`"
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/claude-md
created_at: 2026-01-16T13:43:17Z
updated_at: 2026-01-16T17:12:22Z
url: https://github.com/astral-sh/uv/pull/17517
synced_at: 2026-01-16T18:01:14Z
```

# Add a `CLAUDE.md`

---

_@zanieb_

I'm a bit annoyed to make it tool-specific, and that it's needed in the first place, but 🤷‍♀️ I think it'll make a difference so here we go.

This has a few items inspired by https://github.com/astral-sh/ruff/blob/main/CLAUDE.md

---

_Marked ready for review by @zanieb on 2026-01-16 13:48_

---

_Label `internal` added by @zanieb on 2026-01-16 13:48_

---

_Review requested from @konstin by @zanieb on 2026-01-16 13:53_

---

_Comment by @konstin on 2026-01-16 16:58_

Looks good content-wise.

> I'm a bit annoyed to make it tool-specific

What I've seen in other projects is having an [AGENTS.md](https://agents.md/) and saying `@AGENTS.md` in `CLAUDE.md`, can we use that?

---

_Comment by @zanieb on 2026-01-16 17:11_

I think I'd rather just have one file than two and I think organizationally we're focusing on Claude. Wdyt? I don't have strong feelings.

I sort of don't trust the agent to follow a link to another file. The contents of `CLAUDE.md` are added to each prompt but it doesn't do that if it just points to a file.

---
