```yaml
number: 21869
title: "Documentation: keyword `while` never matches anything (not even a rule name)"
type: issue
state: open
author: Avasam
labels:
  - question
assignees: []
created_at: 2025-12-09T17:58:21Z
updated_at: 2025-12-10T02:03:00Z
url: https://github.com/astral-sh/ruff/issues/21869
synced_at: 2026-01-10T11:10:00Z
```

# Documentation: keyword `while` never matches anything (not even a rule name)

---

_Issue opened by @Avasam on 2025-12-09 17:58_

### Summary

A weird edge case I just found in online doc, typing `while` will never match anything.

<img width="855" height="196" alt="Image" src="https://github.com/user-attachments/assets/71c82996-5920-4138-b591-f016edc88d50" />

<img width="356" height="144" alt="Image" src="https://github.com/user-attachments/assets/57fbf9d1-4793-4020-b77e-cd1d0c46198e" />

<img width="864" height="596" alt="Image" src="https://github.com/user-attachments/assets/5892c054-2462-4bb0-9675-73efa9e22d40" />

### Version

_No response_

---

_Renamed from "Doucmentation: keyword `while` never matches anything (not even a rule name)" to "Documentation: keyword `while` never matches anything (not even a rule name)" by @Avasam on 2025-12-09 18:00_

---

_Comment by @ntBre on 2025-12-09 18:05_

That is strange! I think we just use the built-in mkdocs search [plugin](https://squidfunk.github.io/mkdocs-material/plugins/search/#built-in-search-plugin), though, so I'm not sure how much we can do.

Funnily enough, it also reproduces on those mkdocs docs, so it seems like a problem with the underlying plugin.

---

_Label `question` added by @ntBre on 2025-12-09 18:05_

---

_Comment by @Avasam on 2025-12-09 19:42_

And apparently, MKDocs Material is now in maintenance mode as they focus on something called Zensical ...
- https://squidfunk.github.io/mkdocs-material/changelog/#9.7.0
- https://github.com/squidfunk/mkdocs-material/issues/8523

---

_Comment by @MichaReiser on 2025-12-09 20:00_

Yes, unfortunately, that's the case. We'll need to explore alternative solutions

---

_Comment by @Avasam on 2025-12-10 02:01_

FWIW, it seems other keywords are also excluded. Searching for `if`, `for`, `from` on Ruff's doc will only result in words or blobs that contains the text, but not the text itself.

Any of those keywords give no results on Regular MKDocs as well. https://www.mkdocs.org/ So it may not be specific to MKDocs Material.

<img width="300" alt="Image" src="https://github.com/user-attachments/assets/6f7ccab1-3d17-486b-b6bd-70353f779ca6" />

<img width="300" alt="Image" src="https://github.com/user-attachments/assets/efe74d68-21bb-46ec-8a62-b14d82d2d1d2" />

<img width="200"  alt="Image" src="https://github.com/user-attachments/assets/32e54022-b473-48b2-88aa-77709552be82" />

Anyway, just noting down some observations.

---
