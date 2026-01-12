```yaml
number: 1262
title: "Code completion: filter out objects that are not `BaseException` subclasses after `raise` keyword"
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-09-26T11:53:00Z
updated_at: 2025-11-24T17:55:31Z
url: https://github.com/astral-sh/ty/issues/1262
synced_at: 2026-01-12T15:54:24Z
```

# Code completion: filter out objects that are not `BaseException` subclasses after `raise` keyword

---

_@AlexWaygood_

Quite a common mistake in Python is to accidentally write `raise NotImplemented` rather than `raise NotImplementedError`. The former is not a `BaseException` subclass, so it'll do the wrong thing -- but we currently include it in autocomplete suggestions before `NotImplementedError`

<img width="1668" height="790" alt="Image" src="https://github.com/user-attachments/assets/636c1520-8b01-45e8-8429-b82b26d72de0" />

Since we already know the type of all objects we list in autocomplete suggestions, we could offer much more useful suggestions if we filtered out all names that don't refer to `BaseException` subclasses if the cursor is immediately after a `raise` or `except` keyword

---

_Label `server` added by @AlexWaygood on 2025-09-26 11:53_

---

_Comment by @MichaReiser on 2025-09-26 12:20_

CC: @BurntSushi 

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:48_

---

_Comment by @BurntSushi on 2025-10-02 12:43_

Yeah this sounds like a good idea! I suspect there are more "change completions based on preceding symbol" types of improvements we can make. (We already do that sort of thing for imports, for example.)

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:43_

---

_Removed from milestone `Z post-stable` by @BurntSushi on 2025-11-14 13:35_

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 13:35_

---

_Closed by @BurntSushi on 2025-11-24 17:55_

---
