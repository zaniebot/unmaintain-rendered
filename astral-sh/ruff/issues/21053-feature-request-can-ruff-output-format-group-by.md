```yaml
number: 21053
title: "[Feature request] Can Ruff \"output format\" group by violation code?"
type: issue
state: open
author: jsh9
labels:
  - cli
  - needs-decision
assignees: []
created_at: 2025-10-24T00:38:28Z
updated_at: 2025-10-24T13:49:09Z
url: https://github.com/astral-sh/ruff/issues/21053
synced_at: 2026-01-10T11:10:00Z
```

# [Feature request] Can Ruff "output format" group by violation code?

---

_Issue opened by @jsh9 on 2025-10-24 00:38_

Currently the Ruff linter offer "grouped" as one of the output formats, which groups violations by file.

Can there be another option to group by violation code?

The benefit of this option is: nowadays we often use AI coding assistants (such as Claude Code, Codex, ...) to fix linter issues for us.  I've found that AI performs better when fixing one type of issues at a time.

---

_Comment by @ntBre on 2025-10-24 13:49_

That's interesting. I think I would probably tell the agent to `--select` one rule at a time in that case, but we could still consider it.

---

_Label `cli` added by @ntBre on 2025-10-24 13:49_

---

_Label `needs-decision` added by @ntBre on 2025-10-24 13:49_

---
