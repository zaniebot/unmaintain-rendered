```yaml
number: 1021
title: Show each overload only once in completion suggestions
type: issue
state: open
author: JaRoSchm
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-08-16T15:17:26Z
updated_at: 2026-01-09T15:24:21Z
url: https://github.com/astral-sh/ty/issues/1021
synced_at: 2026-01-12T15:54:24Z
```

# Show each overload only once in completion suggestions

---

_@JaRoSchm_

### Summary

Hi, when using completion for selecting parameters of a function each parameter is shown multiple times if the function has multiple overloads. In principle I understand the reason as different completion items correspond to different overloads and therefore effectively different functions. From a practical point of view however, the information in the completion menu is redundant and I would have to skip multiple suggesting for the same parameter to get to the next possible parameter. Therefore, I'd find it more practical to see each possible parameter only once.

Here is an example for `numpy.linspace`:

<img width="1499" height="304" alt="Image" src="https://github.com/user-attachments/assets/33fcf531-e600-41aa-96e6-4fdf0709a279" />
<img width="1499" height="304" alt="Image" src="https://github.com/user-attachments/assets/13c1f109-7347-4e85-9dde-d48badd556fd" /> 

### Version

ty 0.0.1-alpha.18

---

_Label `server` added by @MichaReiser on 2025-08-16 15:23_

---

_Label `bug` added by @MichaReiser on 2025-08-16 15:23_

---

_Comment by @MichaReiser on 2025-08-16 15:24_

Thanks for reporting this. Makes sense. I'd have to check what other tools do if multiple overloads have the same parameter name but they have different types (and documentation). 

CC: @BurntSushi 

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:49_

---

_Added to milestone `Beta` by @MichaReiser on 2025-11-14 08:46_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-11-14 08:46_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:46_

---

_Renamed from "Show each parameter only once in completion suggestions" to "Show each overload only once in completion suggestions" by @BurntSushi on 2026-01-09 15:21_

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-09 15:24_

---
