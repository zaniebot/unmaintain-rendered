```yaml
number: 1625
title: Inlay hint insert follow ups
type: issue
state: open
author: MatthewMckee4
labels:
  - server
assignees: []
created_at: 2025-11-24T22:55:45Z
updated_at: 2025-12-31T15:36:58Z
url: https://github.com/astral-sh/ty/issues/1625
synced_at: 2026-01-10T01:56:40Z
```

# Inlay hint insert follow ups

---

_Issue opened by @MatthewMckee4 on 2025-11-24 22:55_

Think it's useful to have an issue for the rest of the work that needs done for inlay hint edits.

- [ ] Auto import
- [ ] Don't allow edits for more types.
- [ ] Try to provide some kind of edit for some (currently) disallowed types (callables)

---

_Comment by @MatthewMckee4 on 2025-11-24 23:08_

```py
_CaptureMethod[: <typing.Literal special form>] = Literal["fd", "sys", "no", "tee-sys"]
```

We get this from `KnownInstanceType::repr`. Though the logic its nested in another struct, so I'm unsure how to work around this. Possibly can move this logic to `display.rs` 

---

_Label `server` added by @AlexWaygood on 2025-11-24 23:19_

---

_Comment by @charliermarsh on 2025-12-18 02:23_

Auto import in particular would be great.

---

_Comment by @MatthewMckee4 on 2025-12-19 17:39_

I will have a proper bash at this again

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:36_

---

_Comment by @MichaReiser on 2025-12-31 15:36_

Moving this into pre-stable because of auto imports

---
