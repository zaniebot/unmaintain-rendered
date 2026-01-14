```yaml
number: 22566
title: micha/mdtest no pyi mode
type: pull_request
state: open
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
draft: true
base: main
head: micha/mdtest-no-pyi-mode
created_at: 2026-01-14T09:48:37Z
updated_at: 2026-01-14T10:25:51Z
url: https://github.com/astral-sh/ruff/pull/22566
synced_at: 2026-01-14T10:34:28Z
```

# micha/mdtest no pyi mode

---

_@MichaReiser_

- **Disable pyi mode for mdtests**
- **Reformat files**


---

_Label `testing` added by @MichaReiser on 2026-01-14 09:48_

---

_Label `ty` added by @MichaReiser on 2026-01-14 09:48_

---

_Comment by @MichaReiser on 2026-01-14 09:54_

Doesn't look like the end of the world to me. Yes, it adds quiet a bit of whitespace (about 40 lines per testfile). But it isn't that the files become completely unreadable or twice as long. 

I wouldn't consider it a blocker for adopting ruffendocs but @AlexWaygood might feel differently (we would support `pyi` as language which I think would help with some of the most affected files, as long as they don't test any non-pyi behavior)

---

_Comment by @AlexWaygood on 2026-01-14 10:25_

Definitely not the end of the world, but to me it still feels like a regression in readability/formatting that I'd rather not have :P

I also think the feature more broadly is a reasonable one -- to me it makes sense that you might want your documentation examples formatted more concisely than your production code! You just want to get the semantic meaning of the code across to the reader as quickly as possible in that scenario.

Anyway, I'm curious what others on the ty think too :-)

---
