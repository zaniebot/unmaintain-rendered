```yaml
number: 1298
title: Add Inlay Hints for variables in a for loop statement
type: issue
state: open
author: MatthewMckee4
labels:
  - server
assignees: []
created_at: 2025-10-02T15:52:15Z
updated_at: 2025-11-18T16:10:39Z
url: https://github.com/astral-sh/ty/issues/1298
synced_at: 2026-01-10T01:58:59Z
```

# Add Inlay Hints for variables in a for loop statement

---

_Issue opened by @MatthewMckee4 on 2025-10-02 15:52_

Type hint inlay hints would be nice to have in for loops, like this:

```py

for i[: int] in range(1, 10): ...
```

I think this could be useful and rust-analyzer also supports it.

---

_Label `server` added by @AlexWaygood on 2025-10-02 15:55_

---

_Comment by @CodeMan62 on 2025-10-02 18:56_

I will work on this and propose a PR 

---

_Comment by @MichaReiser on 2025-10-02 19:42_

We should look at what pylance does here and whether it requires a new option 

---

_Comment by @CodeMan62 on 2025-10-03 15:51_

pylance doesn't do this as @MatthewMckee4 is telling it pylance do it something like this 

<img width="796" height="240" alt="Image" src="https://github.com/user-attachments/assets/56bb74c7-3fb8-430d-8dbe-2c19e0aa45d7" />

---

_Comment by @MichaReiser on 2025-10-04 11:48_

Yeah, not sure. I'm leaning towards first improving our inlay hints to make them less verbose (I believe Pylance suppresses them for literals) before adding them in more places. Having said that, I wouldn't be imposed from adding them under the `variable` setting as `i` is a normal variable.

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:41_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
