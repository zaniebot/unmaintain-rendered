```yaml
number: 21284
title: Options to disable global code actions
type: issue
state: closed
author: injust
labels:
  - question
assignees: []
created_at: 2025-11-05T17:39:05Z
updated_at: 2025-11-06T01:21:21Z
url: https://github.com/astral-sh/ruff/issues/21284
synced_at: 2026-01-12T15:54:57Z
```

# Options to disable global code actions

---

_@injust_

Zed shows code actions inline in the buffer. As Ruff provides global "Fix all auto-fixable problems" and "Organize imports" code actions, this makes the code actions button appear on every single line of code (not all at once, but whenever a line is focused, the button appears).

The way Zed surfaces code actions makes it necessary for them to be context-aware, and global code actions don't really fit that pattern. As I use format/autofix-on-save, these code actions are also no-ops most of the time.

I see that https://docs.astral.sh/ruff/editors/settings/#codeaction provides knobs to toggle the context-aware code actions, but not the global ones. It would be good to get a knob for those, because I think the global code actions don't mesh well with format/autofix-on-save.

---

_Label `question` added by @MichaReiser on 2025-11-06 01:00_

---

_Comment by @MichaReiser on 2025-11-06 01:01_

I replied on the issue that you opened in the zed repository. 

With my current understanding, I'm inclined to say that this is an issue specific to Zed and should be fixed in Zed instead of requiring every LSP to special case Zed

---

_Comment by @injust on 2025-11-06 01:21_

Makes sense, thanks for your insight!

---

_Closed by @injust on 2025-11-06 01:21_

---
